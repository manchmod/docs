---
title: "Arista"
date: 2019-10-27T12:32:02-04:00
draft: false
---

## getting vm working on esxi

https://www.arista.com/en/cg-veos-router/veos-router-launching-vmware-esxi-60-and-65

https://eos.arista.com/forum/how-to-boot-veos-combined-on-esxi/


## bgp design

https://the-bitmask.com/2017/08/02/arista-veos-l3slv-part3/

## initial config

https://www.arista.com/assets/data/pdf/user-manual/um-eos/Chapters/Initial%20Configuration%20and%20Recovery.pdf

```
zerotouch cancel (reboots)

login admin

username admin secret pxq123


interface management 1
ip address 192.0.2.8/24
end
copy running-config startup-config
```

