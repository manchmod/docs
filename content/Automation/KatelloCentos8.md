---
title: "Katello CentOS 8"
date: 2019-10-27T15:06:15-04:00
draft: false
--- 


#### CentOS 8 Mirrors nearby

http://mirror.cogentco.com/pub/linux/centos/8/

http://mirror.vtti.vt.edu/centos/8/BaseOS/x86_64/

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

#### Create Content View
```
hammer content-view create --organization-id 1 \
  --name "CentOS_8" \
  --description "Content view for CentOS 8"
```

#### Add Repos to Content View
```
# change $(seq a b) to list from 
hammer repository list --organization-id 1 --product "CentOS 8 Linux for x86_64"

for i in $(seq 1 5); do \
  hammer content-view add-repository --organization-id 1 \
  --name "CentOS_8" \
  --product "CentOS 8 Linux for x86_64" \
  --repository-id "$i"; \
  done
```

#### Publish Content View
```
hammer content-view publish --organization-id 1 \
  --name "CentOS_8" \
  --description "Publishing repositories"
```

#### Create Activation Key
```
hammer lifecycle-environment list --organization-id 1

hammer activation-key create --organization-id 1 \
  --name "centos8" \
  --description "CentOS 8 Activation Key" \
  --lifecycle-environment "Library" \
  --content-view "CentOS_8" \
  --unlimited-hosts

hammer activation-key list --organization-id 1
```

#### Add Subscription to Activation key
```
# list subscriptions for org
hammer subscription list --organization-id 1

# use subscription-id from above
hammer activation-key add-subscription --organization-id 1 \
  --name "centos8" \
  --quantity "1" \
  --subscription-id "2"
