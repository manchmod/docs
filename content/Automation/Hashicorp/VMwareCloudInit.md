---
title: "VMware Cloud-Init"
date: 2019-10-27T15:06:15-04:00
draft: false
---
|Date Added|Description|Link|
|:---|:---|---|
|2019-11-26| CloudInit Docs | https://cloudinit.readthedocs.io/en/latest/v | 
|2019-11-26| Cloud-init for Vsphere | https://blah.cloud/infrastructure/using-cloud-init-for-vm-templating-on-vsphere/|

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

#### Generating $6$ password for cloud-init config
```
mkpasswd --method=SHA-512 --rounds=4096
```

#### Configuring govc

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


#### Winternotch Local 
```
source ~/Dropbox/src/govc-winternotch-vars.sh
govc ls /Winternotch/vm
export VM="/Winternotch/vm/centos7-vm"
export CLOUD_CONFIG=$(gzip -c9 < guesttools/winternotch-cloud-config.yaml | base64)
export METADATA=$(sed 's~NETWORK_CONFIG~'"$(gzip -c9 < guesttools/winternotch-network-config.yaml | \
                    base64)"'~' <guesttools/metadata.json | gzip -9 | base64)
                    
govc vm.change -vm "${VM}" -e guestinfo.metadata="${METADATA}"
govc vm.change -vm "${VM}" -e guestinfo.metadata.encoding=gzip+base64
govc vm.change -vm "${VM}" -e guestinfo.userdata="${CLOUD_CONFIG}"
govc vm.change -vm "${VM}" -e guestinfo.userdata.encoding=gzip+base64
```