---
title: "NSX-T Networking"
date: 2019-10-27T15:41:33-04:00
draft: false
---

## NSX / DVSwitch

https://www.tech-coffee.net/deploy-a-converged-network-with-vsphere-6-5/

## net-dvs

http://buildvirtual.net/utilize-net-dvs-to-troubleshoot-vnetwork-distributed-switch-configurations/


## dv video
https://www.youtube.com/watch?v=_m9PMWrIGyQ

## NSX

http://nxgcloud.com/index.php/2017/03/23/nsx-home-lab-16-deploy-nsx-components/

http://www.vmwarearena.com/vmware-nsx-installation-part-8-configuring-vxlan-on-the-esxi-hosts/

### create logical network segment ID (a critical step)

https://kb.vmware.com/s/article/2081294


## VMWare Networking Info

### Vlan configuration - vmware

https://kb.vmware.com/s/article/1003806

### Esxi network debugging commands

https://blogs.vmware.com/vsphere/2018/12/esxi-network-troubleshooting-tools.html

### Sample VST config

https://kb.vmware.com/s/article/1004074?CoveoV2.CoveoLightningApex.getInitializationData=1&r=3&other.KM_Utility.getArticleDetails=1&other.KM_Utility.getArticleMetadata=1&other.KM_Utility.getUrl=1&other.KM_Utility.getUser=1&other.KM_Utility.getAllTranslatedLanguages=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1


### updating NSX VIBS

http://www.vmwarearena.com/how-to-manually-install-nsx-6-3-0-vibs-on-esxi-6-5-hosts/

https://vswitchzero.com/2018/09/05/manual-upgrade-of-nsx-host-vibs/

### Configure DHCP / Relay

https://letsv4real.com/2017/06/06/configure-dhcp-services-in-vmware-nsx/

### Dynamic Routing / BGP

need to redistribute statics, including allowing default gateways to be pushed from ESG to DLR

http://www.vstellar.com/2018/05/20/configure-and-manage-routing-in-nsx/

https://www.virtually-limitless.com/vcix-nv-study-guide/configure-dynamic-routing-protocols-ospf-bgp-is-is/

### route redistribution

https://docs.vmware.com/en/VMware-NSX-Data-Center-for-vSphere/6.4/com.vmware.nsx.admin.doc/GUID-6DE97E5F-A147-4294-B162-08CB90B2699A.html

weaker explanations
https://blog.bertello.org/2015/02/nsx-for-newbies-part-6-distributed-logical-router-dlr/

older but ok
http://www.routetocloud.com/2014/06/nsx-distributed-logical-router/

### uninstall NSX-V

https://docs.vmware.com/en/VMware-NSX-Data-Center-for-vSphere/6.4/com.vmware.nsx.install.doc/GUID-33D4FCB4-A8C8-4307-B638-AA6BBA0AD2A5.html
