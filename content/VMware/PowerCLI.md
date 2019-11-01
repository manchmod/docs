---
title: "PowerCLI"
date: 2019-10-27T14:02:17-04:00
draft: false
---

### PowerCLI Changelog

https://vdc-download.vmware.com/vmwb-repository/dcr-public/306e3e41-2a54-497e-8377-11a1cf36c107/76c39d63-455d-4f3a-af2a-368f3ea6647c/changelog.html

### Power CLI Users Guide

https://code.vmware.com/docs/7335/powercli-11-0-0-user-s-guide

## Power CLI CMDlet reference

https://code.vmware.com/docs/7336/cmdlet-reference

### PowerCLI Install
```
Install-Module -Name VMWare.PowerCLI
```


### Ignore self signed certs

```
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false
```

### Connect to vcenter

```
connect-viserver -protocol https –server SERVERNAME
```

### Get information on VMs

```
Get-VM | fl | more

Get-VM | where-object {$_.PowerState –eq “PoweredOff”}

Get-VM 〈yourvm〉 | Stop-VMguest
```

### iterate across VMs

```powershell
foreach ($var in $vars){
Do something…
}

Get-vm | where-object {$_.MemoryGB –eq 4 } | select -ExpandProperty Name | out-file c:\VMs.txt
$vms = get-content c:\VMs.txt
Foreach-object ($vm in $vms) {
new-networkadapter -vm $vms -NetworkName "〈Port group name〉" -Type "VMXNET3" –startconnected
}
```




### Links

[5 powercli cmdlets every admin should know](https://www.altaro.com/vmware/5-powercli-cmdlets-every-administrator-know)


[VMware PowerCLI example scripts](https://github.com/vmware/PowerCLI-Example-Scripts/tree/master/Scripts)
