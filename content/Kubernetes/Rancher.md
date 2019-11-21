---
title: "Rancher"
date: 2019-10-27T15:06:15-04:00
draft: false
---

## single node installation

https://rancher.com/docs/rancher/v2.x/en/installation/single-node/

see below for quickstart (assuming docker running on a unix host)

### Persistent Data

Rancher uses etcd as datastore. When using the Single Node Install, the embedded etcd is being used. The persistent data is at the following path in the container: /var/lib/rancher. You can bind mount a host volume to this location to preserve data on the host it is running on.
(put in /opt/rancher, chown'd to user:docker, g+w)

Note that this will NOT work with snap installed versions of docker on ubuntu

```
Launch Command (maps rancher data to local directory)

docker run -d --restart=unless-stopped \
  --name rancher \
  -p 80:80 -p 443:443 \
  -v /opt/rancher:/var/lib/rancher \
  rancher/rancher:latest
```

## manual quick start

https://rancher.com/docs/rancher/v2.x/en/quick-start-guide/deployment/quickstart-manual-setup/

## creating a vpshere cluster

https://rancher.com/docs/rancher/v2.x/en/cluster-provisioning/rke-clusters/node-pools/vsphere/


Error with pre-create check: "host 'Compute' not found"; Timeout waiting for ssh key

"Host" setting wrong. Need to leave blank for Cluster with DRS.

Error with pre-create check: "default resource pool resolves to multiple instances, please specify"; Timeout waiting for ssh key

Need to create a Resource Pool for this

## updating rancher docker image

https://rancher.com/docs/rancher/v2.x/en/upgrades/upgrades/single-node/

- ensure data is properly externalized

```
docker ps 
docker stop <imagename>
docker pull rancher/rancher:latest
docker start <imagename>
```