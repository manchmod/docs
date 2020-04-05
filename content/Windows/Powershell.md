---
title: "Powershell"
date: 2019-10-27T15:06:15-04:00
draft: false
---


## mac powershell

https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6

```
brew tap caskroom/cask 
brew cask install powershell
```

### Windows Events

[Get-WinEvent](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7)

[Get-Eventlog](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-eventlog?view=powershell-5.1)

https://blogs.technet.microsoft.com/ashleymcglone/2013/08/28/powershell-get-winevent-xml-madness-getting-details-from-event-logs/

https://letitknow.wordpress.com/2012/09/02/powershell-and-the-applications-and-services-logs/


```

# GroupPolicy Examples
Get-EventLog -LogName Application | Where-Object {$_.EventId -eq 6006}
Get-EventLog -LogName Application | Where-Object {$_.EventId -eq 8000}
Get-EventLog -LogName Application | Where-Object {$_.EventId -eq 8001}
Get-EventLog -LogName Application | Where-Object {$_.EventId -eq 8006}
Get-EventLog -LogName Application | Where-Object {$_.EventId -eq 8007}

Get-WinEvent -FilterHashTable @{ LogName = "Microsoft-Windows-GroupPolicy/Operational";  ID = 4016 } -MaxEvents 10

Get-WinEvent -FilterHashTable @{ LogName = "Microsoft-Windows-GroupPolicy/Operational";  ID = 5312 } -MaxEvents 3  | fl

(other eventIDs - 4016,5016,5317, etc)

# CAPI Examples
https://github.com/jajp777/powershell-scripts-1/blob/master/Enable-Capi2Logging.ps1

 Get-WinEvent -FilterHashTable @{ LogName = "Microsoft-Windows-CAPI2/Operational"; LevelDisplayName = Error} -MaxEvents 100


(Get-WinEvent -ListProvider Microsoft-Windows-CAPI2).Events |
   Where-Object {$_.Id -eq 41}
            
# For a keyword in the event data            
(Get-WinEvent -ListProvider Microsoft-Windows-GroupPolicy).Events |            
   Where-Object {$_.Template -like "*reason*"}            
            
# Find an event ID across all ETW providers:            
Get-WinEvent -ListProvider * |            
   ForEach-Object { $_.Events | Where-Object {$_.ID -eq 4168} }    


```

### string searches (a.k.a. grep )

https://www.thomasmaurer.ch/2011/03/powershell-search-for-string-or-grep-for-powershell/

```
Select-String
```

### getting credentials to pass

https://ss64.com/ps/get-credential.html
```
Get-Credential
```