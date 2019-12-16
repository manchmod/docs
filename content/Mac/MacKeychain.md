---
title: "Mac Keychain"
date: 2019-12-15T13:18:17-04:00
draft: false
---

|Date Added|Description|Link|
|:---|:---|---|
|2019-11-24| Jamf Adding Keychain Discussion| https://www.jamf.com/jamf-nation/discussions/22294/adding-a-certificate-to-the-system-keychain-set-to-always-trust| 


### Mac KeyChain Stuff


```
# grab SSL cert in DER format
openssl s_client -showcerts -connect HOST443 </dev/null 2>/dev/null|openssl x509 -outform DER > /tmp/cerfificate.der

# add to keychain
/usr/bin/security -v add-trusted-cert -r trustAsRoot -e hostnameMismatch -d -p ssl -p  smime -p  codeSign -p  IPSec  -p basic -p  swUpdate -p  pkgSign -p eap -p timestamping -u -1 -k /Users/gnotch/Library/Keychains/login.keychain-db cerfificate.der
```

### Other Greasy Options
```
# Login Keychain 
/usr/bin/security -v add-trusted-cert -r trustRoot -e hostnameMismatch -d -k /Users/gnotch/Library/Keychains/login.keychain-db certificate.pem

# System Keychain Trustroot
/usr/bin/security -v add-trusted-cert -r trustRoot -e hostnameMismatch -d -k /Library/Keychains/System.keychain /tmp/certificate.der


```
