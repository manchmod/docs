---
title: "GCP Setup"
date: 2019-10-27T15:06:15-04:00
draft: false
---

## installing SDK (Includes CLI)

https://cloud.google.com/sdk/docs/

### linux
```
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
```

### mac

download sdk 

https://cloud.google.com/sdk/docs/#mac

run install.sh


## Basic configuration

``` 
gcloud init

gcloud config configurations list

```

zone us-east4-a




