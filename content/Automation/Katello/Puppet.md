---
title: "Puppet"
date: 2019-10-27T15:06:15-04:00
draft: false
---

https://www.puppetcookbook.com/


### check client status
```
/opt/puppetlabs/bin/puppet agent --test

```


### Install some modules
```
cd /opt/puppetlabs/bin
./puppet module list
./puppet module install puppetlabs/ntp
./puppet module install puppetlabs/motd
./puppet module install puppetlabs/accounts
./puppet module install saz-sudo

```

### specify environment
```
puppet module --environment test list
```

### validate (on server)
```
puppet parser validate site.pp
```
