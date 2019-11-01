---
title: "Upgrading"
date: 2019-10-27T15:06:15-04:00
draft: false
---

### VMware files

https://datavak.eu/vmware/

### VMware patch tracker

https://esxi-patches.v-front.de/ESXi-6.7.0.html#2018-10-16

### VMware Upgrade

https://vmiss.net/2018/10/17/upgrade-esxi-67-u1-vmware-vsphere-update-manager/

https://virtualizationreview.com/articles/2018/10/22/upgrading-to-vcsa-67u1.aspx


### Updating from CLI

(in this case the Jan 2019 version of 6.7.0U1)

```
# Cut and paste these commands into an ESXi shell to update your host with this Imageprofile
# See the Help page for more instructions
#
esxcli network firewall ruleset set -e true -r httpClient
esxcli software profile update -p ESXi-6.7.0-20190104001-standard \
-d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml
esxcli network firewall ruleset set -e false -r httpClient
#
# Reboot to complete the upgrade
```