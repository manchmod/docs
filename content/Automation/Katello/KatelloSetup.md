---
title: "Katello Setup"
date: 2019-10-27T15:06:15-04:00
draft: false
---

https://theforeman.org/plugins/katello/3.13/installation/index.html

http://www.outsidaz.org/2017/02/12/adventures-in-katello-part-1/


### Preflight

- The machine needs at least 8G of ram and 2 VCPU (12 and 4 recommended)

- The path /var/spool/squid/ is used as a temporary location for some types of repository syncs and may grow to consume 10s of GB of space before the files are migrated to /var/lib/pulp. You may wish to put this on the same partition as /var/lib/pulp.

- TL;DR create big external volumes for /var/spool/squid &/var/lib/pulp


#### selinux
```
# allow read on /var/www/html
chcon -R -h -t httpd_sys_content_t /var/www/html

# YOLO

/etc/selinux/config
SELINUX=disabled
reboot

#verify
getenforce

```

#### firewall
```
firewall-cmd --list-ports
firewall-cmd --list-services


firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --permanent --add-port=5000/tcp
firewall-cmd --permanent --add-port=5646-5647/tcp
firewall-cmd --permanent --add-port=5671/tcp
firewall-cmd --permanent --add-port=8000/tcp
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --permanent --add-port=8140/tcp
firewall-cmd --permanent --add-port=9090/tcp
firewall-cmd --permanent --add-port=67-69/udp
firewall-cmd --permanent --add-port=53/udp
firewall-cmd --permanent --add-port=53/tcp
firewall-cmd --reload

# YOLO

systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld

```

#### Install Prep (Assumes EPEL already there)
```
sudo -s
yum -y localinstall https://fedorapeople.org/groups/katello/releases/yum/3.13/katello/el7/x86_64/katello-repos-latest.rpm
yum -y localinstall https://yum.theforeman.org/releases/1.23/el7/x86_64/foreman-release.rpm
yum -y localinstall https://yum.puppet.com/puppet6-release-el-7.noarch.rpm
yum -y install foreman-release-scl
yum -y update
```

(20191123) on a Centos7 image, this last part broke due to some dependencies for libsolv on a newer version by pulp. 

```
yum remove libsolv-0.6.34-4.el7.x86_64
yum install  libsolv-0.7.3-3.el7.x86_64
yum -y update 
```

This resolves the dependency problem by updating libsolv, However, this uninstalls dnf

#### Katello Install


```
yum -y install katello

# list install options
foreman-installer --scenario katello --help

# install with defaults
foreman-installer --scenario katello 

# interactive install 

foreman-installer --scenario katello -i 

# The full log is at /var/log/foreman-installer/katello.log

```

#### Additional Proxies 

- Katello is running at https://foreman.winternotch.com
- To install an additional Foreman proxy on separate machine continue by running:
```
foreman-proxy-certs-generate --foreman-proxy-fqdn "$FOREMAN_PROXY" --certs-tar "/root/$FOREMAN_PROXY-certs.tar"
```
