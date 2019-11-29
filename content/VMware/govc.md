---
title: "govc"
date: 2019-10-27T15:06:15-04:00
draft: false
---

|Date Added|Description|Link|
|:---|:---|---|
|2019-11-26| Govc Usage| https://github.com/vmware/govmomi/blob/master/govc/USAGE.md| 



## Setup 

#### installing

`brew tap govmomi/tap/govc && brew install govmomi/tap/govc`

note that this might not be the latest version. my homebrew only had 0.19 which notably didn't contain the library commands. I just grabbed the latest release from here: https://github.com/vmware/govmomi/releases and put it in ~/bin

#### Configuring govc for use
```
source govc-vars.sh
$ alias govc=$(pwd)/govc_darwin_amd64
$ export GOVC_URL=vcenter.foo.com
$ export GOVC_USERNAME=x@x.com
$ export GOVC_PASSWORD=password
$ export GOVC_INSECURE=1
```

## Usage

### VM

#### list vms
`govc ls /datacenter/vm/`

#### get vm info
`govc vm.info /datacenter/vm/new-vm`


#### clone vm 
```
  govc vm.clone -vm template-vm new-vm
  govc vm.clone -vm template-vm -link new-vm
  govc vm.clone -vm template-vm -snapshot s-name new-vm
  govc vm.clone -vm template-vm -link -snapshot s-name new-vm
  govc vm.clone -vm template-vm -snapshot $(govc snapshot.tree -vm template-vm -C) new-vm
```

#### clone template from library 
```
  govc library.deploy /library_name/ovf_template vm_name
  govc library.deploy /library_name/ovf_template -options deploy.json
```

#### export ovf

`govc export.ovf -vm $vm DIR`

#### import spec file from ovf
```
govc import.spec

govc import.spec file.ova | python -m json.tool > ubuntu.json

govc import.spec file.ova | jq > spec.json
```


#### change vm size
`govc vm.change -vm vm_name -c 4 -m 4096`

#### open VM console in VMRC (MacOS)
`open $(govc vm.console my-vm)`

#### list processes in vm
```
 govc guest.ps -vm $name
  govc guest.ps -vm $name -e
  govc guest.ps -vm $name -p 12345
  govc guest.ps -vm $name -U roo
```

### Vsphere Commands 

#### get event log
`govc events`




