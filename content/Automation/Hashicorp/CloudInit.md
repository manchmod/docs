---
title: "Cloudinit"
date: 2019-10-27T15:06:15-04:00
draft: false
---

### basics

directory layout

https://cloudinit.readthedocs.io/en/latest/topics/dir_layout.html

### debugging
https://cloudinit.readthedocs.io/en/latest/topics/debugging.html

```
# grab logs 
cloud-init collect-logs

# analyze cloud init
cloud-init analyze show -i /var/log/cloud-init.log

# see what vmtoolsd sent

vmtoolsd --cmd "info-get guestinfo.userdata"


```
