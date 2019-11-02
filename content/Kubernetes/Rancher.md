---
title: "Rancher"
date: 2019-10-27T15:06:15-04:00
draft: false
---

## single node installation

https://rancher.com/docs/rancher/v2.x/en/installation/single-node/


- configure rancher config externalization from docker container


## updating rancher docker image

https://rancher.com/docs/rancher/v2.x/en/upgrades/upgrades/single-node/

- ensure data is properly externalized

```
docker ps 
docker stop <imagename>
docker pull rancher/rancher:latest
docker start <imagename>
```