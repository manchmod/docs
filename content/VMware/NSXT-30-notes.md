---
title: "NSX-T 3.0 Notes"
date: 2019-10-27T15:41:33-04:00
draft: false
---

## good install guides

https://docs.pivotal.io/tkgi/1-8/nsxt-3-0-install.html

https://virtualrove.com/2020/06/02/nsx-t-3-0-series-part7-add-a-tier-0-gateway-and-configure-bgp-routing/

http://www.vstellar.com/2020/08/07/nsx-t-3-0-series-part-5-configure-logical-routing/#Create_Uplink_For_T0_Gateway

uses manual "manager" install, so technically not 100% compliant, manually adds T0 components, etc. 

Useful Steps
	- create edge node, with switches for overlay and vlan
	- node transport profiles with same for hosts
	- ip address for edge in vlan network
	- static default route


Note you still must add an additional switch (1 for overlay, 1 for vlan) when creating an edge node and manually assign it to an fp interface. this interface must correspond to the edge vm's NICs in Vcenter, and be on the correct vlans. 

## setting up Postman API

https://rutgerblom.com/2019/06/16/getting-started-with-the-nsx-t-api-and-postman/

## Set up transport zone via VDS

https://rutgerblom.com/2020/04/08/nsx-t-3-0-meets-vsphere-7-vds-7-0/


## command line cheat sheet

https://www.simongreaves.co.uk/nsx-t-command-line-cheat-sheet/

