---
title: "Windows 10"
date: 2019-10-27T14:02:17-04:00
draft: false
---

### creating a local windows 10 account
https://www.groovypost.com/howto/create-local-account-windows-10/


### sysprep for windows vm image

```
 %windir%\system32\sysprep\sysprep.exe /oobe /generalize /shutdown /mode:vm
```

https://www.thomasmaurer.ch/2016/05/windows-sysprep-for-virtual-machines/


### extend a volume


[launching disk management snapin](https://www.isunshare.com/windows-10/7-ways-to-open-disk-management-in-windows-10.html)

```
open elevated cmd.exe
diskmgmt.msc
```

[partition article](https://www.howtogeek.com/101862/how-to-manage-partitions-on-windows-without-downloading-any-other-software/)

right click on existing partition and select "extend", and follow wizard.


### windows 10 updater download

https://www.microsoft.com/en-us/software-download/windows10


### Troubleshooting GPO (and speed)

https://4sysops.com/archives/troubleshooting-group-policy-part-4-client-problems/

http://woshub.com/troubleshoot-slow-gpo-processing-and-login-speed-impact/
