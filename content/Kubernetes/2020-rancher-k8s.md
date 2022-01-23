---
title: "2020 rancher k8s notes"
date: 2020-12-19
draft: false
---

# build k8s cluster for rancher 
[https://rancher.com/docs/rancher/v2.x/en/installation/resources/k8s-tutorials/infrastructure-tutorials/infra-for-ha-with-external-db/](https://rancher.com/docs/rancher/v2.x/en/installation/resources/k8s-tutorials/infrastructure-tutorials/infra-for-ha-with-external-db/)

# mysql database server

[https://computingforgeeks.com/install-mysql-5-7-on-centos-rhel-linux/](https://computingforgeeks.com/install-mysql-5-7-on-centos-rhel-linux/)

dnf remove @mysql
dnf module reset mysql && sudo dnf module disable mysql
vi /etc/yum.repos.d/mysql-community.repo
sudo dnf config-manager --disable mysql80-community\n
sudo dnf config-manager --enable mysql57-community\n
dnf install mysql-community-server
sudo systemctl enable --now mysqld.service\n
sudo grep 'A temporary password' /var/log/mysqld.log |tail -1\n
mysql_secure_installation
mysql -u root -p



firewall-cmd --zone=public --list-all
firewall-cmd --zone=public --add-port=3306/tcp
firewall-cmd --runtime-to-permanent

CREATE DATABASE rancher

CREATE USER 'rancher'@'localhost' IDENTIFIED BY 'Rancher-k3s!';
CREATE USER 'rancher'@'%' IDENTIFIED BY 'Rancher-k3s!';

GRANT ALL ON rancher.* TO 'rancher'@'localhost';
GRANT ALL ON rancher.* TO 'rancher'@'%';

# 2 linux nodes

curl -sfL https://get.k3s.io | sh -s - server \
--datastore-endpoint="mysql://rancher:Rancher-k3s\!@tcp(rancher-mysql:3306)/rancher" \
--tls-san 10.4.70.150 \
--tls-san rancher.winternotch.com

> host firewalls are for fags (centos8 isnt quite there)

```
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

## set up load balancer
- point DNS to IP
- LB to both nodes, port 80,443, 6443


## kubectl

export KUBECONFIG=/Users/gnotch/src/k3s-vmware/rancher-k3s.yaml
kubectl get node -o wide

> cert issue

Unable to connect to the server: x509: certificate is valid for 10.4.70.151, 10.4.70.152, 10.43.0.1, 127.0.0.1, not 10.4.70.150
kubectl --insecure-skip-tls-verify get node

[https://github.com/rancher/k3s/issues/1381](https://github.com/rancher/k3s/issues/1381)

[https://rancher.com/docs/k3s/latest/en/installation/install-options/#registration-options-for-the-k3s-server](https://rancher.com/docs/k3s/latest/en/installation/install-options/#registration-options-for-the-k3s-server)

[https://rancher.com/docs/k3s/latest/en/installation/install-options/server-config/](https://rancher.com/docs/k3s/latest/en/installation/install-options/server-config/)


fixed by alteringa install and adding --tls-san or by adding IP of LB to 

/etc/rancher/k3s/config.yaml
```
write-kubeconfig-mode: "0644"
tls-san:
  - "10.4.70.150"
```

# install rancher with selfsigned certs 

[https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/](https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/)

- going with the rancher generated certificates, so need to install cert-manager

```
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest

kubectl create namespace cattle-system

kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml

kubectl create namespace cert-manager

helm repo add jetstack https://charts.jetstack.io

helm repo update

helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.0.4

kubectl get pods --namespace cert-manager

helm install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=rancher.winternotch.com

```

W1127 22:20:08.452706    1046 warnings.go:67] extensions/v1beta1 Ingress is deprecated in v1.14+, unavailable in v1.22+; use networking.k8s.io/v1 Ingress
W1127 22:20:08.508988    1046 warnings.go:67] extensions/v1beta1 Ingress is deprecated in v1.14+, unavailable in v1.22+; use networking.k8s.io/v1 Ingress


random ssl error, gotta get the CA cert out of the rancher UI in firefox and add to local store on mac (dynamiclistener-ca)

socket.js:50 WebSocket connection to 'wss://rancher.winternotch.com/v3/clusters/local/subscribe?sockId=2' failed: Error in connection establishment: net::ERR_CONNECTION_REFUSED


# cert manager
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.0.4

In order to begin issuing certificates, you will need to set up a ClusterIssuer
or Issuer resource (for example, by creating a 'letsencrypt-staging' issuer).

More information on the different types of issuers and how to configure them
can be found in our documentation:

https://cert-manager.io/docs/configuration/

For information on how to configure cert-manager to automatically provision
Certificates for Ingress resources, take a look at the `ingress-shim`
documentation:

https://cert-manager.io/docs/usage/ingress/


# install rancher with internal CA certs
> PREREQ: generate certificates and sign them with the CA. Ensure SANs for hostnames and IPs are added.

> Using the "Certificates From Files" option from this link
[https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/#](https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/#)

add TLS certs as speficied here
[https://rancher.com/docs/rancher/v2.x/en/installation/resources/encryption/tls-secrets/](https://rancher.com/docs/rancher/v2.x/en/installation/resources/encryption/tls-secrets/)


```
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest

kubectl create namespace cattle-system

# add key and certificate (rename to tls.crt and tls.key)
kubectl -n cattle-system create secret tls tls-rancher-ingress \
  --cert=tls.crt \
  --key=tls.key

# add CA cert (rename to cacerts.pem)
kubectl -n cattle-system create secret generic tls-ca \
  --from-file=cacerts.pem=./cacerts.pem

helm install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=rancher.winternotch.com \
  --set ingress.tls.source=secret \
  --set privateCA=true

# check to make sure install goes ok
kubectl -n cattle-system rollout status deploy/rancher

```

# uninstall and reinstall

> you need -n for the namespaces....
helm install rancher
helm uninstall cert-manager

> f' it ill just delete the namespaces
> these hang sometimes 
kubectl delete namespace cattle-system
kubectl delete namespace cert-manager
kubectl delete namespace fleet-system

> wacking k3s on the nodes to start from known clean
/usr/local/bin/k3s-killall.sh
/usr/local/bin/k3s-uninstall.sh


# Switching between namespaces

[https://nikgrozev.com/2019/10/03/switch-between-multiple-kubernetes-clusters-with-ease/](https://nikgrozev.com/2019/10/03/switch-between-multiple-kubernetes-clusters-with-ease/)

```
kubectl config get-contexts

kubectl config current-context

kubectl config use-context NikTest
```


## fresh k3s namespace
```
/usr/local/bin/k3s kubectl get all --all-namespaces

NAMESPACE     NAME                                         READY   STATUS      RESTARTS   AGE
kube-system   pod/local-path-provisioner-7ff9579c6-8qdwb   1/1     Running     0          14m
kube-system   pod/metrics-server-7b4f8b595-jmwtq           1/1     Running     0          14m
kube-system   pod/helm-install-traefik-n9jdh               0/1     Completed   0          14m
kube-system   pod/svclb-traefik-72zrn                      2/2     Running     0          14m
kube-system   pod/svclb-traefik-xbxlt                      2/2     Running     0          14m
kube-system   pod/traefik-5dd496474-fxl9v                  1/1     Running     0          14m
kube-system   pod/coredns-66c464876b-cptxj                 1/1     Running     0          14m

NAMESPACE     NAME                         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
default       service/kubernetes           ClusterIP      10.43.0.1       <none>        443/TCP                      14m
kube-system   service/kube-dns             ClusterIP      10.43.0.10      <none>        53/UDP,53/TCP,9153/TCP       14m
kube-system   service/metrics-server       ClusterIP      10.43.144.165   <none>        443/TCP                      14m
kube-system   service/traefik-prometheus   ClusterIP      10.43.235.90    <none>        9100/TCP                     14m
kube-system   service/traefik              LoadBalancer   10.43.233.242   10.4.70.151   80:31246/TCP,443:31762/TCP   14m

NAMESPACE     NAME                           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
kube-system   daemonset.apps/svclb-traefik   2         2         2       2            2           <none>          14m

NAMESPACE     NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/local-path-provisioner   1/1     1            1           14m
kube-system   deployment.apps/metrics-server           1/1     1            1           14m
kube-system   deployment.apps/coredns                  1/1     1            1           14m
kube-system   deployment.apps/traefik                  1/1     1            1           14m

NAMESPACE     NAME                                               DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/local-path-provisioner-7ff9579c6   1         1         1       14m
kube-system   replicaset.apps/metrics-server-7b4f8b595           1         1         1       14m
kube-system   replicaset.apps/coredns-66c464876b                 1         1         1       14m
kube-system   replicaset.apps/traefik-5dd496474                  1         1         1       14m

NAMESPACE     NAME                             COMPLETIONS   DURATION   AGE
kube-system   job.batch/helm-install-traefik   1/1           17s        14m


```


Loosely, my understanding Kubernetes works like this:

You have a Pod definition, which is basically a gang of containers. In that Pod definition you have included one or more container image references.

You send the Pod definition to the API Server.

The API Server informs listeners for Pod updates that there is a new Pod definition. One of these listeners is the scheduler, which decides which Node should get the Pod. It creates an update for the Pod's "status", essentially annotating it with the name of the Node that should run the Pod.

Each Node has a "kubelet". These too subscribe to Pod definitions. When a change shows up saying "Pod 'foo' should run on Node 27", the kubelet in Node 27 perks up its ears.

The kubelet converts the Pod definition into descriptions of containers -- image reference, RAM limits, which disks to attach etc. It then turns to its container runtime through the "Container Runtime Interface" (CRI). In the early days this was a Docker daemon.

The container runtime now acts on the descriptions it got. Most notably, it will check to see if it has an image in its local cache; if it doesn't then it will try to pull that image from a registry.

Now: The CRI is distinct from the Docker daemon API. The CRI is abstracted because since the Docker daemon days, other alternatives have emerged (and some have withered), such as rkt, podman and containerd.

This update says "we are not going to maintain the Docker daemon option for CRI". You can use containerd. From a Kubernetes end-user perspective, nothing should change. From an operator perspective, all that happens is that you have a smaller footprint with less attack surface. 


## Rancher Articles

Rancher on k8s

[https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/#](https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/#)

HA Install
[https://rancher.com/docs/rancher/v2.x/en/installation/resources/k8s-tutorials/ha-with-external-db/](https://rancher.com/docs/rancher/v2.x/en/installation/resources/k8s-tutorials/ha-with-external-db/)

Eternal DB for HA
[https://rancher.com/docs/rancher/v2.x/en/installation/resources/k8s-tutorials/infrastructure-tutorials/infra-for-ha-with-external-db/](https://rancher.com/docs/rancher/v2.x/en/installation/resources/k8s-tutorials/infrastructure-tutorials/infra-for-ha-with-external-db/)

TLS  Secrets
[https://rancher.com/docs/rancher/v2.x/en/installation/resources/encryption/tls-secrets/](https://rancher.com/docs/rancher/v2.x/en/installation/resources/encryption/tls-secrets/)

