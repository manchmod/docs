---
title: "Jetbrains Packer vSphere"
date: 2019-10-27T15:06:15-04:00
draft: false
---


|Date Added|Description|Link|
|:---|:---|---|
|2019-11-24| Jetbrains Packer Builder Vsphere| https://github.com/jetbrains-infra/packer-builder-vsphere| 



#### building
(requires docker)
```
git clone https://github.com/jetbrains-infra/packer-builder-vsphere
cd packer-builder-vsphere
docker-compose run build
```

#### install plugins
https://www.packer.io/docs/extending/plugins.html#installing-plugins
```
cd packer-builder-vsphere
mkdir -p $HOME/.packer.d/plugins
cp bin/*.macos $HOME/.packer.d/plugins
```

#### example configs

https://github.com/jetbrains-infra/packer-builder-vsphere/tree/master/examples/

#### centos examples (unmerged pulls)

centos8 : https://github.com/nelg/packer-builder-vsphere

centos7 : https://github.com/remijouannet/packer-builder-vsphere

### running packer

```
packer build -force centos7-hardened-esx.json

# debug logging
PACKER_LOG=1 packer build -force centos7-hardened-esx.json
```
