---
title: "Hashicorp Vault"
date: 2019-10-27T15:06:15-04:00
draft: false
---

### Vault Commands

#### start server
```
vault server -config=/Users/gnotch/Dropbox/vault/vault.hcl
```

#### Ignore TLS validation
```
export VAULT_SKIP_VERIFY=true

vault -tls-skip-verify
```

#### create vault

https://www.vaultproject.io/docs/commands/operator/init.html

```
vault operator init 

# only make 3 unseal keys and only require one of them to unseal (sane for single user)
vault operator init \
    -key-shares=3 \
    -key-threshold=1 \

# encrypt vault keys with GPG 
vault operator init \
    -key-shares=3 \
    -key-threshold=1 \
    -pgp-keys="/Users/gnotch/Dropbox/crt/yubi-20180814-public-gpg.txt"
```
#### unseal vault & login
(by default, 3x with 3 diff unseal keys)
```
vault operator unseal
vault login
```

#### using secrets
```
# create path + secrets engine
vault secrets enable -path=data kv

# put
vault kv put data/hello foo=world
vault kv put data/hello foo=world bar=yes

# get
vault kv get data/hello

```

#### Ignore TLS validation
```
export VAULT_SKIP_VERIFY=true

vault -tls-skip-verify
```

#### Vault Config File

```
storage "file" {
	path = "/Users/gnotch/Dropbox/vault/vault-db"
}

listener "tcp" {
 address     = "127.0.0.1:8200"
 tls_disable = 0
 tls_cert_file = "/Users/gnotch/Dropbox/vault/vault.crt"
 tls_key_file = "/Users/gnotch/Dropbox/vault/vault.key"
}

```


