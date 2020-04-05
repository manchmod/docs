---
title: "Windows Server"
date: 2019-10-27T14:02:17-04:00
draft: false
---


## TSS Debug Script

https://github.com/CSS-Windows/WindowsDiag/tree/master/ALL/TSS


#### setting up NTP server in Windows Server
https://support.microsoft.com/en-us/help/816042/how-to-configure-an-authoritative-time-server-in-windows-server

w32tm command

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ff799054(v%3Dws.11)

### How credential processing works

https://docs.microsoft.com/en-us/windows-server/security/windows-authentication/credentials-processes-in-windows-authentication

https://www.ultimatewindowssecurity.com/securitylog/book/page.aspx?spid=chapter3


#### window scaling tuning 

https://www.duckware.com/blog/how-windows-is-killing-internet-download-speeds/index.html

### List filter Drivers (WSC)

https://ss64.com/nt/fltmc.html


### List windows drivers

https://www.thewindowsclub.com/list-device-drivers-driverquery-command-prompt

```
Get-WindowsDriver -Online

driverquery /v | Select-String “running”
```
