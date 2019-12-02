---
title: "Katello CentOS 7"
date: 2019-10-27T15:06:15-04:00
draft: false
--- 

### Initial Setup

#### create product
```
hammer product create --organization-id 1 \
  --name "CentOS 7 Linux for x86_64" \
  --description "Repositories to use with CentOS 7 Linux"
```
#### import key
```
hammer gpg create --organization-id 1 \
  --key "RPM-GPG-KEY-CentOS-7" \
  --name "RPM-GPG-KEY-CentOS-7"
```

http://mirror.cogentco.com/pub/linux/centos/7/os/x86_64/


### Add Repos

#### BaseOS
```
hammer repository create --organization-id 1 \
  --product "CentOS 7 Linux for x86_64" \
  --name "CentOS 7 Base RPMS" \
  --label "CentOS_7_Base_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-7" \
  --url "http://mirror.cogentco.com/pub/linux/centos/7/os/x86_64/" \
  --mirror-on-sync "no"
```


#### Updates
```
hammer repository create --organization-id 1 \
  --product "CentOS 7 Linux for x86_64" \
  --name "CentOS 7 Update RPMS" \
  --label "CentOS_7_Update_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-7" \
  --url "http://mirror.cogentco.com/pub/linux/centos/7/updates/x86_64/" \
  --mirror-on-sync "no"
```


#### Extras
```
hammer repository create --organization-id 1 \
  --product "CentOS 7 Linux for x86_64" \
  --name "CentOS 7 Extras RPMS" \
  --label "CentOS_7_Extras_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-7" \
  --url "http://mirror.cogentco.com/pub/linux/centos/7/extras/x86_64/" \
  --mirror-on-sync "no"
```

sclo repo: http://mirror.cogentco.com/pub/linux/centos/7/sclo/x86_64/

#### EPEL
```
hammer repository create --organization-id 1 \
  --product "CentOS 7 Linux for x86_64" \
  --name "CentOS 7 EPEL RPMS" \
  --label "CentOS_7_EPEL_RPMS" \
  --content-type "yum" \
  --download-policy "on_demand" \
  --gpg-key "RPM-GPG-KEY-CentOS-7" \
  --url "http://mirror.cogentco.com/pub/linux/epel/7/x86_64/" \
  --mirror-on-sync "no"
```


#### Create Content View
```
hammer content-view create --organization-id 1 \
  --name "CentOS_7" \
  --description "Content view for CentOS 7"
```

#### Add Repos to Content View
```
# change $(seq a b) to list from 
hammer repository list --organization-id 1 --product "CentOS 7 Linux for x86_64"

for i in $(seq 6 8); do \
  hammer content-view add-repository --organization-id 1 \
  --name "CentOS_7" \
  --product "CentOS 7 Linux for x86_64" \
  --repository-id "$i"; \
  done
```

#### Publish Content View
```
hammer content-view publish --organization-id 1 \
  --name "CentOS_7" \
  --description "Publishing repositories"
```

#### Create Activation Key
```
hammer lifecycle-environment list --organization-id 1

hammer activation-key create --organization-id 1 \
  --name "centos7" \
  --description "CentOS 7 Activation Key" \
  --lifecycle-environment "Library" \
  --content-view "CentOS_7" \
  --unlimited-hosts

hammer activation-key list --organization-id 1
```

#### Add Subscription to Activation key
```
# list subscriptions for org
hammer subscription list --organization-id 1

# use subscription-id from above
hammer activation-key add-subscription --organization-id 1 \
  --name "centos7" \
  --quantity "1" \
  --subscription-id "3"
```
