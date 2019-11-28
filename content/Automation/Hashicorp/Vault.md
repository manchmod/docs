---
title: "Hashicorp Vault"
date: 2019-10-27T15:06:15-04:00
draft: false
---
|Date Added|Description|Link|
|:---|:---|---|
|2019-11-26| VaultEnv | https://github.com/channable/vaultenv | 

### Setup

#### start server
```
vault server -config=/Users/gnotch/Dropbox/vault/vault.hcl
```

#### unseal vault & login
(by default, 3x with 3 diff unseal keys)
```
vault operator unseal
vault login
```

#### Ignore TLS validation
```
export VAULT_SKIP_VERIFY=true

vault -tls-skip-verify
```

#### VAULT_TOKEN
vault login just creates `~/.vault-token`
If you want other processes to use it from the einvronment, you must put the value of the token into env var with

`export VAULT_TOKEN=`

```
export VAULT_TOKEN=$(cat ~/.vault-token)
```


### Operations


#### using vaults
```
# create path + secrets engine
vault secrets enable -path=winternotch kv

# list secrets
vault secrets list
vault secrets list -detailed

```

#### using secrets
```

# put
vault kv put winternotch/test foo=no
vault kv put winternotch/test foo=yes bar=no

# get
vault kv get winternotch/test 

# list 
vault kv list winternotch/

```

#### packer variable to key mapping
```
# key storage
vault kv put secret/hello foo=world

# packer variable
{
  "variables": {
    "my_secret": "{{ vault `/secret/data/hello` `foo`}}"
  }
}

# example
vault kv put winternotch/vsphere_username value="administrator@winternotch.com"
"vsphere_username": "{{ vault `winternotch/data/vsphere_username` `value`}}",

```


#### create vault

https://www.vaultproject.io/docs/commands/operator/init.html

```
vault operator init 

# only make 1 unseal keys and only require one of them to unseal (sane for single user)
vault operator init \
    -key-shares=1 \
    -key-threshold=1 

# encrypt vault keys with GPG 
vault operator init \
    -key-shares=3 \
    -key-threshold=1 \
    -pgp-keys="/Users/gnotch/Dropbox/crt/yubi-20180814-public-gpg.txt"
```