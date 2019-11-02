---
title: "VSphere / ESXi"
date: 2019-10-27T15:57:57-04:00
draft: false
---

## vmware home lab

http://techgenix.com/vmware-home-lab/


### Vsan 2 node

https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/vcat/vmware-virtual-san-two-node-architecture-service-provider-use-cases.pdf

### VCenter CA Certs

https://esxsi.com/2017/09/11/vcenter-ca-certs

### Vmotion

https://kb.vmware.com/s/article/2054994


### setting web client timeout 

https://kb.vmware.com/s/article/2040626


### VSphere CLI reference

https://code.vmware.com/docs/6676/vsphere-command-line-interface-reference


### setting maintenance mode from CLI

syntax slightly wrong for 6.7 but basics correct
https://www.vladan.fr/esxi-maintenance-mode/


### vrni

https://www.virtualizationhowto.com/2019/01/installing-vrealize-network-insight-4-0-platform-and-proxy-appliances/


### fixing 10G ethernet problem on smesxi
```
esxcli network nic down -n vmnic0
esxcli network nic up -n vmnic0

[root@smesxi:~] esxcli network nic set --speed 1000 --duplex full -n vmnic2
[root@smesxi:~] esxcli network nic set --speed 1000 --duplex full -n vmnic3

vsish -e set /net/pNics/vmnic3/linkSet 1

vmkload_mod -u drivername
vmkload_mod drivername

vmware cli - create documentation

https://github.com/arielsanchezmora/vDocumentation

### instant clones

https://www.virtuallyghetto.com/2018/04/new-instant-clone-architecture-in-vsphere-6-7-part-1.html
https://www.virtuallyghetto.com/2018/04/new-instant-clone-architecture-in-vsphere-6-7-part-2.html
http://www.yellow-bricks.com/2018/05/01/instant-clone-vsphere-67/
```

### import VMDKs and convert

https://kb.vmware.com/s/article/1028943
```
vmkfstools -i HostedVirtualDisk ESXVirtualDisk
```

Where HostedVirtualDisk is the path to the vmdk on the host and ESXVirtualDisk is the vmdk to be output by the command.

For example:
```
vmkfstools -i /vmfs/volumes/datastore/virtual_machine_folder/ virtual_machine.vmdk /vmfs/volumes/datastore/new_virtual_machine_folder/ virtual_machine.vmdk
```

### make copy/paste work from individual ESXi VM

https://kb.vmware.com/s/article/57122
```
isolation.tools.copy.disable FALSE
isolation.tools.paste.disable FALSE
isolation.tools.setGUIOptions.enable TRUE
```

### vcenter root password reset
https://www.kieri.com/how-to-fix-or-change-vcenter-root-password-expired-6-5-and-6-7/

### network adapter disappears from windows VM

https://kb.vmware.com/s/article/1020718






