---
title: "SSL / Certificate Authority"
date: 2019-10-27T13:18:17-04:00
draft: false
---

|Date Added|Description|Link|
|:---|:---|---|
|2019-10-27|Smart Card / HSM Backed CA | https://framkant.org/2018/04/smartcard-hsm-backed-openssl-ca/ |
|2019-10-27|ECDSA PKI | https://tools.ietf.org/id/draft-moskowitz-ecdsa-pki-01.html|
|2019-10-27|Jamielinux guide |https://jamielinux.com/docs/openssl-certificate-authority/index.html?origin_team=TFEM4UUP7|
|2019-9-23|PanOS Cert Management|https://www.paloaltonetworks.com/documentation/71/pan-os/pan-os/certificate-management/obtain-a-certificate-from-an-external-ca|
|2019-8-14|xca quick start|https://blogs.sap.com/2017/06/26/how-to-guide-xca-quick-start-guide/|
|2019-8-14|common ssl commands|https://www.sslshopper.com/article-most-common-openssl-commands.html|
|2019-8-14|openssl quick reference|https://www.digicert.com/ssl-support/openssl-quick-reference-guide.htm|
|2019-8-14|soarez gist| https://gist.github.com/Soarez/9688998|


### Generate TLS Certificate
```
openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out MyCertificate.crt -keyout MyKey.key
```


### Generate4 TLS Certificate with SAN

#### Config file (san.cnf)
```
[req]
default_bits = 2048
prompt = no
default_md = sha256
x509_extensions = v3_req
distinguished_name = dn

[dn]
C = ES
ST = MyState
L = MyCity
O = MyOrg
emailAddress = email@mydomain.com
CN = mydomain.com

[v3_req]
subjectAltName = @alt_names

[alt_names]
DNS.1 = mydomain.com
DNS.2 = www.mydomain.com
```

and for IP based SAN
```
[alt_names]
IP.1 = 127.0.0.1
```

#### Command
```
openssl req -new -x509 -newkey rsa:2048 -sha256 -nodes -keyout my.key -days 3560 -out my.crt -config san.cnf
```
