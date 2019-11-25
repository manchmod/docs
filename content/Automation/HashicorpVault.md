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

#### unseal vault & login
(3x with 3 diff keys)
```
vault operator unseal
```

#### using secrets
```
# login
vault login

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


