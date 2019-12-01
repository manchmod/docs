---
title: "Vanilla Packer ESX Scripts"
date: 2019-10-27T15:06:15-04:00
draft: false
---


|Date Added|Description|Link|
|:---|:---|---|
|2019-11-24| vmware-iso Packer Docs | https://www.packer.io/docs/builders/vmware-iso.html |
|2019-11-24| VMware Cloud-Init GuestInfo | https://github.com/vmware/cloud-init-vmware-guestinfo | 
|2019-11-24| Configuring ESXi Prereqs | https://blog.ukotic.net/2019/03/05/configuring-esxi-prerequisites-for-packer/|

## VMware Packer Scripts for CentOS

#### code is available here https://git.notch.org/notch/vmware-automation/tree/master/vmware-packer

### NOTE: While this technique and code works, I'm would strongly suggest dropping this in favor of using the packer plugins and scripts that Jetbrains has on github. See the article on that in this section. The information here is still useful if you are interested in how this all works.


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

### CAVEATS - README! 
- this ONLY works against an esxi host and not vcenter
- for some reason this only works against local storage in the esxi host. vmkfstools appears to make a truncated vmdk when used against NFS, at least in my environment.
- you cannot use a distributed port group for the network, those can only be used by vcenter. You must use a local portgruop.

### requirements


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


