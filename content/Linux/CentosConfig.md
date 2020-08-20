---
title: "Centos Config"
date: 2019-10-27T15:53:35-04:00
draft: false
---

## set up network/hostname

```
nmtui
```

## Remove EPEL

```
yum remove epel-release
```

## Restore Main Config
```
sys-unconfig
```

## Disable Firewall
```
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

## Configure Firewall

https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7


## Python 3.7 install
```
yum install gcc openssl-devel bzip2-devel libffi-devel
cd /usr/src
curl -O https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tgz
tar zxf Python-3.7.2.tgz
./configure --enable-optimizations
make -j4 altinstall
```

## NFS tools
```
yum install nfs-utils
```

## SELinux

https://wiki.centos.org/HowTos/SELinux/

## Install Webmin

http://www.webmin.com/rpm.html
```
 sudo yum -y install perl perl-Net-SSLeay openssl perl-IO-Tty perl-Encode-Detect unzip perl-Data-Dumper
 rpm -U [webmin RPM]
 firewall-cmd --zone=public --add-port=10000/tcp
```
