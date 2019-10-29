---
title: "Cisco 3850"
date: 2019-10-27T12:32:02-04:00
draft: false
---

## wireshark on switch

https://www.cisco.com/c/en/us/support/docs/switches/catalyst-3850-series-switches/117639-configure-wireshark-00.html

### capture reference
https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/epc/command/epc-cr-book/epc-cr-m1.html#wp1398253443


## mac tftp server

https://rick.cogley.info/post/run-a-tftp-server-on-mac-osx/


## config fragments

```
!
vlan 99
 name 99
!

interface GigabitEthernet1/0/11
 description smesxi-vmnic1
 switchport mode trunk

3850(config)#int Gi1/0/11
3850(config-if)#switchport trunk allowed vlan all
```


## trunking

```
interface GigabitEthernet1/0/23
 description ubnt24-uplink2
 switchport mode trunk
 channel-group 1 mode active
!
interface GigabitEthernet1/0/24
 description ubnt24-uplink1
 switchport mode trunk
 channel-group 1 mode active
!
```