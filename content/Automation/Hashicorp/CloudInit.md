---
title: "Cloudinit"
date: 2019-10-27T15:06:15-04:00
draft: false
---

### basics

directory layout

https://cloudinit.readthedocs.io/en/latest/topics/dir_layout.html

### UserData and Metadata

config files passed in as gzip compressed base64 encoded strings from terraform or govc

[cyberchef](https://gchq.github.io/CyberChef) is your friend for debugging these.
 

### debugging
https://cloudinit.readthedocs.io/en/latest/topics/debugging.html

```
# grab logs 
cloud-init collect-logs

# analyze cloud init
cloud-init analyze show -i /var/log/cloud-init.log

# see what vmtoolsd sent

vmtoolsd --cmd "info-get guestinfo.userdata"

#
cloud-init --debug init

cloud-init --debug modules -m config

```
