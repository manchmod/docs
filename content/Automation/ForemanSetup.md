---
title: "Foreman Setup"
date: 2019-10-27T15:06:15-04:00
draft: false
---

### Foreman Quick Install

## NOTE: You cannot install Katello over Foreman. This is for standalone only

https://theforeman.org/manuals/1.23/quickstart_guide.html

```
sudo yum -y install https://yum.puppet.com/puppet6-release-el-7.noarch.rpm

sudo yum -y install https://yum.theforeman.org/releases/1.23/el7/x86_64/foreman-release.rpm

sudo yum -y install foreman-installer

sudo foreman-installer
```


### Foreman Plugins Install

List Plugins available to install
```
sudo foreman-installer --help | grep enable-
```

#### compute plugins

--enable-foreman-compute-vmware 
--enable-foreman-compute-ec2 
--enable-foreman-compute-gce
--enable-foreman-plugin-digitalocean 

#### other plugins
--enable-foreman-plugin-ansible 
--enable-foreman-plugin-openscap




