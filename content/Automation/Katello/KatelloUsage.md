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

hammer repository synchronize --organization-id 1 --product "CentOS 8 Linux for x86_64" --id 1


# confirm sync state

hammer product list  --name "CentOS 8 Linux for x86_64" --organization-id 1

```


#### Sync Repos
```
# centos8 
for i in $(seq 1 5); do \
  hammer repository synchronize --async --organization-id 1 \
  --product "CentOS 8 Linux for x86_64" \
  --id "$i"; \
  done

# centos7
for i in $(seq 6 8); do \
  hammer repository synchronize --async --organization-id 1 \
  --product "CentOS 7 Linux for x86_64" \
  --id "$i"; \
  done
```

#### Client 
```
# upload initial eratta
katello-package-upload -f
```




