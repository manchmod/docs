---
title: "Docker Setup"
date: 2019-10-27T15:06:15-04:00
draft: false
---

## installing docker on ubuntu 18.04

### DO NOT USE UBUNTU SNAP, SNAP NOT SUPPORTED ANY MORE (20191117)

guide: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04

```
(setup)
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update

(check to make sure docker will be installed from the right place)
apt-cache policy docker-ce

(install)
sudo apt install docker-ce
sudo systemctl status docker
```
