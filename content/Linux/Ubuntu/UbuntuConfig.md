---
title: "Ubuntu Config"
date: 2019-10-27T15:53:35-04:00
draft: false
---

## set hostname

```
sudo hostnamectl set-hostname linuxconfig
```

Futhermore, check for the existence of /etc/cloud/cloud.cfg configuration. If the file exists edit the file and change the settings within:

```
FROM:
preserve_hostname: false
TO:
preserve_hostname: true
```

## reset hostid for dhcp

https://unix.stackexchange.com/questions/419321/why-are-my-cloned-linux-vms-fighting-for-the-same-ip

systemd-networkd uses a different method to generate the DUID than dhclient. dhclient by default uses the link-layer address while systemd-networkd uses the contents of /etc/machine-id. Since the VMs were cloned, they have the same machine-id and the DHCP server returns the same IP for both.
To fix, replace the contents of one or both of /etc/machine-id. This can be anything, but deleting the file and running systemd-machine-id-setup will create a random machine-id in the same way done on machine setup.

restart networking 

netplan apply

```
iface eth0 inet static
    address 10.0.0.41
    netmask 255.255.255.0
    network 10.0.0.0
    broadcast 10.0.0.255
    gateway 10.0.0.1
    dns-nameservers 10.0.0.1 8.8.8.8
    dns-domain acme.com
    dns-search acme.com
```


## enable universe repo

https://askubuntu.com/questions/148638/how-do-i-enable-the-universe-repository


## Enable RDP for desktop

[c-nergy.be blog](https://c-nergy.be/blog/?p=13390)

[xrdp on ubuntu 18.04 LTS](https://askubuntu.com/questions/1031519/xrdp-on-ubuntu-18-04lts)

[xrdp issues](https://github.com/neutrinolabs/xrdp/issues/598)


## install docker
```
snap install docker
sudo addgroup --system docker
sudo adduser notch docker
newgrp docker
```