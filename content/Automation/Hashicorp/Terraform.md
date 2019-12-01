---
title: "Hashicorp Terraform"
date: 2019-10-27T15:06:15-04:00
draft: false
---
|Date Added|Description|Link|
|:---|:---|---|
|2019-11-26| Terraform Docs | https://www.terraform.io/docs|
|2019-11-29|tf 0.12 release notes | https://discuss.hashicorp.com/t/terraform-0-12-14-released/3898|
|2019-11-29| Vault Provider | https://www.terraform.io/docs/providers/vault/index.html|
|2019-11-29| Vault Generic Secret Provider | https://www.terraform.io/docs/providers/vault/d/generic_secret.html|
|2019-11-29| Terraform clone on vsphere| https://sdorsett.github.io/post/2018-12-24-using-terraform-to-clone-a-virtual-machine-on-vsphere/ | 
|2019-11-29|tf issues | https://itnext.io/things-i-wish-i-knew-about-terraform-before-jumping-into-it-43ee92a9dd65|
|2019-11-29|local and remote exec provisioners |https://sdorsett.github.io/post/2018-12-26-using-local-exec-and-remote-exec-provisioners-with-terraform/|
|2019-11-29|simple k8s example | https://github.com/vmware/simple-k8s-test-env/blob/master/e2e/vsphere.tf|
|2019-12-1|terraform course examples|https://github.com/wardviaene/terraform-course/blob/master/demo-10/instance.tf|
|2019-12-1|terraform and vsphere with cloudinit examples|https://floating.io/2019/04/iaas-terraform-and-vsphere/|

### main configuration files

main.tf

variables.tf

outputs.tf

tfvars


### running
```
terraform init

terraform plan
TF_LOG=TRACE terraform plan

terraform apply -auto-approve

terraform destroy -auto-approve

```


#### fffffuuu

interpolation only syntax changed in 0.12 (20191113)

also, fucking strings vs integers

a.k.a no quotes on values in either the tfvars or the .tf file. beacuse fuck you thats why.

https://discuss.hashicorp.com/t/terraform-0-12-14-released/3898
https://www.terraform.io/upgrade-guides/0-12.html#referring-to-list-variables

also that ^M shit in tmux terminal. wtf.

terraform with vault is fucking broken, generic_secret requires a namespace? wtf?

https://github.com/terraform-providers/terraform-provider-vault/issues/470

cloning vm image with larger size breaks

Error: error reconfiguring virtual machine: error reconfiguring virtual machine: File  is larger than the maximum size supported by datastore ''

https://kb.vmware.com/s/article/1012384

### cloudinit notes


some example stuff:

https://github.com/terraform-providers/terraform-provider-vsphere/issues/588
https://github.com/akutz/yake2e/blob/master/vsphere.tf#L102
https://github.com/akutz/yake2e/blob/master/cloud_config.tf

### notes

even standalone esxi servers have resource pools, as do vcenter ones...

https://www.terraform.io/docs/providers/vsphere/d/resource_pool.html

```
data "vsphere_resource_pool" "pool" {
  name          = "esxi1/Resources"
  datacenter_id = "${data.vsphere_datacenter.dc.id}"
}
```

whats the difference? is this why partitioning broke?
 
 expected scsi_type to be one of [pvscsi lsilogic lsilogic-sas] (pvscsi best)


whats the difference?

expected network_interface.0.adapter_type to be one of [e1000 e1000e vmxnet3],
