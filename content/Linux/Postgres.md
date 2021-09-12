+++
title = "Postgres"
description = "Postgresql configuration"
+++

## install postgres

https://www.tecmint.com/install-postgressql-and-pgadmin-in-centos-8/

## allow remote connection
https://bigbinary.com/blog/configure-postgresql-to-allow-remote-connection

```
edit postgresql.conf (allow connections)
edit pg_hba.conf (allow connections all)

firewall-cmd --permanent --zone public --add-port 80/tcp
firewall-cmd --permanent --zone public --add-port 443/tcp
firewall-cmd --reload
firewall-cmd --permanent --zone public --add-port 5432/tcp
```


## pgadmin

https://computingforgeeks.com/how-to-install-pgadmin-4-on-centos-linux/



