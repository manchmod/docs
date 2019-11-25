---
title: "ip command"
date: 2019-10-27T15:53:35-04:00
draft: false
---

for those of us used to ifconfig for 25 years....

#### show ip interfaces
```
# equivalent to ifconfig -a
ip addr show
```

#### add/remove ip address
```
ip addr add 192.168.50.5 dev eth1
ip addr del 192.168.50.5/24 dev eth1
```

#### default gateway
```
ip route add default via 192.168.50.100
```

#### link up
```
ip link set eth1 up
ip link set eth1 down
```

#### routes
```
ip route show
ip route add 10.10.20.0/24 via 192.168.50.100 dev eth0
ip route del 10.10.20.0/24
```

