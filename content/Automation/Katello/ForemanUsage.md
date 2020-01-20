---
title: "Foreman Usage"
date: 2019-10-27T15:06:15-04:00
draft: false
---

[Foreman Manual](https://theforeman.org/manuals/1.23/index.html#1.Foreman1.23Manual)

[cstan - orphan task cleanup](https://cstan.io/?p=8976&lang=en)


### Foreman Plugins

List Plugins available to install
```
sudo foreman-installer --help | grep enable-
```

#### adding plugins
```
# can be added post install, eg 
foreman-installer --enable-foreman-compute-vmware

--enable-foreman-compute-vmware 
--enable-foreman-compute-ec2 
--enable-foreman-compute-gce
--enable-foreman-plugin-digitalocean 

--enable-foreman-plugin-ansible 
--enable-foreman-plugin-openscap
```


### foreman debugging

```
# basic health check
foreman-maintain health check

```

#### postgres commands

```
# tasks
su - postgres -c "psql foreman -c 'select * from foreman_tasks_tasks'"

# locks
 su - postgres -c "psql foreman -c 'select * from foreman_tasks_locks'"

# really wide query, su to postgres and do this

PAGER="less -S"  psql foreman -c 'select * from foreman_tasks_tasks'

```

#### rake commands

```
foreman-rake katello:upgrade_check

# clean task log
foreman-rake foreman_tasks:cleanup TASK_SEARCH="" AFTER=1d

#resync db, useful after task cleanup
foreman-rake katello:reimport


# rake console
foreman-rake console

# delete task by ID, from rake console
ForemanTasks::Task.find("e140a579-a4a2-4a5f-9c32-97b6ae3700e8").destroy


```



### Puppet
```

puppet agent --test
puppet module install puppetlabs/ntp

```

------

### Foreman Quick Install

#### NOTE: You cannot install Katello over Foreman. This is for standalone only

https://theforeman.org/manuals/1.23/quickstart_guide.html

```
sudo yum -y install https://yum.puppet.com/puppet6-release-el-7.noarch.rpm

sudo yum -y install https://yum.theforeman.org/releases/1.23/el7/x86_64/foreman-release.rpm

sudo yum -y install foreman-installer

sudo foreman-installer
```
