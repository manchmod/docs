---
title: "Centos JetBrains Packer"
date: 2019-10-27T15:06:15-04:00
draft: false
---


|Date Added|Description|Link|
|:---|:---|---|
|2019-11-24| Jetbrains Packer Builder Vsphere| https://github.com/jetbrains-infra/packer-builder-vsphere| 
|2019-11-24| VMware Guestinfo Cloud-Init| https://github.com/vmware/cloud-init-vmware-guestinfo/releases|

## Building a Hardened Centos VM Template using Packer 

We are going to build for centos-7, which is not one of the examples that Jetbrains provides, but there are several pull requests on the github repo which contain some examples to work from. We're going to use packer to build a centos vm from a base ISO using kickstart, configure it further using cloud-init, and finally use packer provisioners to run some hardening scripts against it. 

##### my centos7 implementation is here: https://git.notch.org/notch/vmware-automation/tree/master/winternotch-hardened-centos7

### Theory of Operation 

##### Packer Builder
This is the main packer scipt. The first part executes now (the builder part), and the rest executes after the image is built by kickstart and cloud-init (the provisioner part). Note that we use vault to store the credentials in the configuration file 

- downloads the base ISO to packer_cache locally, validates the checksum, 
- takes the kickstart file (ks.cfg) and the cloud-init script (cloud-init.json) and builds a floppy image
- logs into vcenter, and copies the iso and floppy image up to the data store specified.
- configures the vm profile according to the parameters in the packer JSON
- mounts the media (ISO and floppy image) on the VM
- boots the vm and begins the installation

##### Kickstart Config (ks.cfg)
This is a redhat format unattended install script which does the following:

- Boots unattended install of Centos
- Configures disk geometry, etc.
- Installs packages from the ISO
- Configures the network (this is a temp config)
- enables sshd, turns on the firewall
- Adds the centos user with centos password (more on this later)
- installs the cloud-init RPM
- installs the special vmware-cloud-init-guestinfo RPM. 
	NOTE: you need a yum repo to serve this up. I store it on a katello server but really any yum repo will do, or you can point directly to the github repo if your environment has internet access
	`https://github.com/vmware/cloud-init-vmware-guestinfo/releases/download/v1.1.0/cloud-init-vmware-guestinfo-1.1.0-1.el7.noarch.rpm`
- Copies the initial cloud-init script. This is very important because the default cloud-init that comes with the rpm by default uses dhcp for IPs (which is fine, but doesnt allow it to work in non dhcp environments) Even more importantly it disables password based logins, which will prevent packer from being able to log into the vm and complete its provisioning run later. So we take this opportunity to lay down our own initial cloudinit config so we can control what happens on the next boot
- once all of this is complete, kickstart ejects the removable media and reboots the machine

##### Cloud-Init Script
This is the bootstrap cloud init script. Once kickstart reboots the machine and it comes up, cloud-init takes over the configuration of the machine.
	
- reconfigures centos user, adds a ssh key and maintains the centos password. really this will only ever be used to debug the image and the image creation process, which should be done in a controlled environment, so this is OK. Later on in the process we will remove this user entirely, probably with puppet.
- disables the root user entirely
- removes default sshd keys and certificates
- sets up the network interface again

It can do a lot more. There are seveal other things that should go here. Honestly you could probably put all of the rest of the stuff that gets executed by packer provisioners (see below) in the cloud-init script if you wanted to. 
- yum repo configuration (point to internal repos)
- puppet rpm installation from that repo
- any other configuration management

##### Packer Provisioner
This is the "provisioner" part of the Packer script. What we use this for is to ssh into the image as the centos user and execute a series of shell scripts as root using sudo. The reason we do it this way is because we've already locked the root user down in previous steps. These scripts are designed to run on the image just prior to it being shut down, and handle genericization of the image. This includes

- SCAP lockdown and other OS hardening (auditd rules, selinux etc.)
- remove any UUIDS, and reset MAC addresses so that the image is vanilla and unique when cloned 
- remove any log files or any other traces of the install process 
- remove lingering ssh keys for the centos user. (we still want the password for console access until the machine enters normal configuration management )
- disable password authentication for sshd
- zero out the filesystem so the image is smaller (optional)
- put final desired version of cloud-init configuration in place

Once this is complete, packer shuts down the image, and turns it into a template in vcenter that you can clone.

### Prerequistites
- vcenter server
- esxi host (note that no ssh or vnc should be required on the host, you just need the name of one in the cluster to build on)
- hashicorp vault 
	- setup with credentials for vcenter server
- packer
- jetbrains packer plugins installed in $HOME/.packer.d/plugins



#### building jetbrains packer plugins (recommended)
(requires docker)
```
git clone https://github.com/jetbrains-infra/packer-builder-vsphere
cd packer-builder-vsphere
docker-compose run build
```

#### install plugins
https://www.packer.io/docs/extending/plugins.html#installing-plugins
```
cd packer-builder-vsphere
mkdir -p $HOME/.packer.d/plugins
cp bin/*.macos $HOME/.packer.d/plugins
```

#### example configs

https://github.com/jetbrains-infra/packer-builder-vsphere/tree/master/examples/

#### centos examples (unmerged pulls)

centos8 : https://github.com/nelg/packer-builder-vsphere

centos7 : https://github.com/remijouannet/packer-builder-vsphere

### running packer

```
# normal build
packer build winternotch-centos7.json

# force continuation if artifacts exist
packer build -force centos7-hardened-esx.json

# dont destroy vm on abort (to debug errors)
packer build -on-error=abort winternotch-centos7.json

# debug logging
PACKER_LOG=1 packer build -force centos7-hardened-esx.json

# step by step debugging 
packer build -debug winternotch-centos7.json
```
