---
title: "Packer ESX Scripts"
date: 2019-10-27T15:06:15-04:00
draft: false
---


|Date Added|Description|Link|
|:---|:---|---|
|2019-11-24| notch.org vmware automation | https://git.notch.org/notch/vmware-automation |
|2019-11-24| VMware Cloud-Init GuestInfo | https://github.com/vmware/cloud-init-vmware-guestinfo | 

## VMware Packer Scripts for CentOS


### Overview 

A packer template and associated scripts to provision a hardened CentOS 7 build

https://www.packer.io/intro/

Output images include cloud-init package to initialize system on first launch (cloud-init.sh)
  - resize partitions
  - change and lock default centos user
  - set up initial users to allow ssh key-based access in case of puppet failure (to be cleaned up/overwritten with puppet once the first check-in succeeds)
  - check in with puppet

A Makefile is included - to build: `make build`

This script currently leaves the machine built on ESXi server, it can be exported as OVF, or VMX/VMDK

More info here: https://www.packer.io/docs/builders/vmware-iso.html

In its final state, this script should be run from or on a machine that can provision guests using DHCP or an expected static range isolated from the rest of our network

This script currently builds machines with static IP addresses - this should be templated so we can re-use a single OVF and dynamically set IP before boot

### requirements
https://blog.ukotic.net/2019/03/05/configuring-esxi-prerequisites-for-packer/

NOTE: this works against an esxi host and not vcenter

- ssh enabled on the esxi host
- govc (`brew install govmomi/tap/govc`)
- ovftool (can be part of Fusion, tools at /Applications/VMware\ Fusion.app/Contents/Library/)
- packer 
- web server
    - ks.cfg
    - vmware-cloud-init rpm
- GuestIPHack is required, enable by running this
`esxcli system settings advanced set -o /Net/GuestIPHack -i 1`

##### enable VNC  in the ESXi firewall (5900-6000)
```
chmod 644 /etc/vmware/firewall/service.xml
chmod +t /etc/vmware/firewall/service.xml
vi /etc/vmware/firewall/service.xml
```

add this right before /ConfigRoot
```
<service id="1000">
  <id>packer-vnc</id>
  <rule id="0000">
    <direction>inbound</direction>
    <protocol>tcp</protocol>
    <porttype>dst</porttype>
    <port>
      <begin>5900</begin>
      <end>6000</end>
    </port>
  </rule>
  <enabled>true</enabled>
  <required>true</required>
</service>
```

```
chmod 444 /etc/vmware/firewall/service.xml
esxcli network firewall refresh

# list rules
esxcli network firewall ruleset list
esxcli network firewall ruleset rule list

```


### running packer

```
packer build -force centos7-hardened-esx.json

# debug logging
PACKER_LOG=1 packer build -force centos7-hardened-esx.json
```


### guest-tools

https://github.com/vmware/cloud-init-vmware-guestinfo

Providing vmware metadata support for cloud-init via vmtoolsd

```
vmtoolsd --cmd 'info-get /vm/path'
```

3 configuration files are required:

- network configuration file
- metadata file
- cloud-config file

Examples are in guesttools dir


### Assigning the cloud-config data to the VM's GuestInfo

Guestinfo can be assigned to VMs using the vmware-cmd command on the console, or a tool like PowerCLI or govc.

https://tech.lazyllama.com/2010/06/22/passing-info-from-powercli-into-your-vm-using-guestinfo-variables/


#### Configuring govc
(source env var file)

```
$ alias govc=$(pwd)/govc_darwin_amd64
$ export GOVC_URL=vcenter.foo.com
$ export GOVC_USERNAME=x@x.com
$ export GOVC_PASSWORD=password
$ export GOVC_INSECURE=1
```

#### Find VM with ls
```
$ govc ls
$ govc ls /datacenter/
$ govc ls /datacenter/vm/
$ export VM="/datacenter/vm/vm-name"
```

#### Create files and set env variables
```
$ cd guesttools
$ export CLOUD_CONFIG=$(gzip -c9 <cloud-config.yaml | base64)
$ export METADATA=$(sed 's~NETWORK_CONFIG~'"$(gzip -c9 <network.config.yaml | \
                    base64)"'~' <metadata.json | gzip -9 | base64)
```

#### Assign vm metadata
```
$ govc vm.change -vm "${VM}" -e guestinfo.metadata="${METADATA}"
$ govc vm.change -vm "${VM}" -e guestinfo.metadata.encoding=gzip+base64
$ govc vm.change -vm "${VM}" -e guestinfo.userdata="${CLOUD_CONFIG}"
$ govc vm.change -vm "${VM}" -e guestinfo.userdata.encoding=gzip+base64
```

#### Power on VM


#### Winternotch GOVC 
```
source ~/Dropbox/src/govc-winternotch-vars.sh
govc ls /Winternotch/vm
export VM="/Winternotch/vm/Base Images/centos7-base"
export CLOUD_CONFIG=$(gzip -c9 < guesttools/winternotch-cloud-config.yaml | base64)
export METADATA=$(sed 's~NETWORK_CONFIG~'"$(gzip -c9 < guesttools/winternotch-network-config.yaml | \
                    base64)"'~' <guesttools/metadata.json | gzip -9 | base64)
                    
govc vm.change -vm "${VM}" -e guestinfo.metadata="${METADATA}"
govc vm.change -vm "${VM}" -e guestinfo.metadata.encoding=gzip+base64
govc vm.change -vm "${VM}" -e guestinfo.userdata="${CLOUD_CONFIG}"
govc vm.change -vm "${VM}" -e guestinfo.userdata.encoding=gzip+base64
```