---
title: "Katello Usage"
date: 2019-10-27T15:06:15-04:00
draft: false
---

### Katello commands
```
# stop katello
katello-service stop

# start katello
katello-service start

# main production log

tail -f /var/log/foreman/production.log

```

### Hammer commands 
```
# list org, get ID
hammer organization list

# list products 
hammer product list --organization-id 1

# list repositories for product

hammer repository list --organization-id 1 --product "CentOS 8 Linux for x86_64"

# sync repos

hammer repository synchronize --async --organization-id 1 \
--product "CentOS 8 Linux for x86_64" \
--id "$i"; 

# confirm sync state

hammer product list  --name "CentOS 8 Linux for x86_64" --organization-id 1


```

### Adding Centos 8 Repository

https://computingforgeeks.com/sync-centos-8-repositories-on-satellite-katello-foreman/

#### Initial Setup 
```
# Create CentOS 8 Product 
hammer product create --organization-id 1 \
  --name "CentOS 8 Linux for x86_64" \
  --description "Repositories to use with CentOS 8 Linux"

# import centos PKI
mkdir /etc/pki/rpm-gpg/import/
cd /etc/pki/rpm-gpg/import/
wget https://www.centos.org/keys/RPM-GPG-KEY-CentOS-Official

hammer gpg create --organization-id 1 \
  --key "RPM-GPG-KEY-CentOS-Official" \
  --name "RPM-GPG-KEY-CentOS-8"
```

#### Add Repos

```
# BaseOS
hammer repository create --organization-id 1 \
  --product "CentOS 8 Linux for x86_64" \
  --name "CentOS 8 Base RPMS" \
  --label "CentOS_8_Base_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-8" \
  --url "http://mirror.cogentco.com/pub/linux/centos/8/BaseOS/x86_64/os/" \
  --mirror-on-sync "no"

# AppStream
hammer repository create --organization-id 1 \
  --product "CentOS 8 Linux for x86_64" \
  --name "CentOS 8 AppStream RPMS" \
  --label "CentOS_8_AppStream_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-8" \
  --url "http://mirror.cogentco.com/pub/linux/centos/8/AppStream/x86_64/os/" \
  --mirror-on-sync "no"

# PowerTools
hammer repository create --organization-id 1 \
  --product "CentOS 8 Linux for x86_64" \
  --name "CentOS 8 PowerTools RPMS" \
  --label "CentOS_8_PowerTools_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-8" \
  --url "http://mirror.cogentco.com/pub/linux/centos/8/PowerTools/x86_64/os/" \
  --mirror-on-sync "no"

# CentosPlus
hammer repository create --organization-id 1 \
  --product "CentOS 8 Linux for x86_64" \
  --name "CentOS 8 centosplus RPMS" \
  --label "CentOS_8_centosplus_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-8" \
  --url "http://mirror.cogentco.com/pub/linux/centos/8/centosplus/x86_64/os/" \
  --mirror-on-sync "no"

# Extras
hammer repository create --organization-id 1 \
  --product "CentOS 8 Linux for x86_64" \
  --name "CentOS 8 extras RPMS" \
  --label "CentOS_8_extras_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-8" \
  --url "http://mirror.cogentco.com/pub/linux/centos/8/extras/x86_64/os/" \
  --mirror-on-sync "no"

```

#### Sync Repos
```
# list repos up and put them in the sequence 
for i in $(seq 64 68); do \
  hammer repository synchronize --async --organization-id 1 \
  --product "CentOS 8 Linux for x86_64" \
  --id "$i"; \
  done
```

#### CentOS Mirrors close to me

http://mirror.cogentco.com/pub/linux/centos/8/

http://mirror.vtti.vt.edu/centos/8/BaseOS/x86_64/


