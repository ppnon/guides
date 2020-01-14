# Vault

## Instalation (docker)

```bash
# default run
docker run --name vault -p 8200:8200 --restart=always --cap-add=IPC_LOCK -d vault

# run developer mode
docker run --name dev-vault -p 8200:8200 --restart=always --cap-add=IPC_LOCK -e 'VAULT_ADDR=http://127.0.0.1:8200' -e 'VAULT_DEV_ROOT_TOKEN_ID=mysecrettoken' -d vault server -dev

# run production mode
docker run --name prod-vault -p 8200:8200 --restart=always --cap-add=IPC_LOCK -e 'VAULT_LOCAL_CONFIG={"backend": {"file": {"path": "/vault/file"}}, "listener": {"tcp": {"address": "0.0.0.0:8200",	"tls_disable": 1}}, "ui": true}' -d vault server
```
Check installation
```bash
dokcer ps | grep dev-vault
docker logs dev-vault

# access container
docker exec -ti dev-vault sh

# inside de container
vault status
```

## AppRole use (developer mode)

role-id es equivalente al username
secret-id es equivalente al password

```bash
# login (use VAULT_DEV_ROOT_TOKEN_ID)
vault login

# list secrets engines enabled
vault secrets list

# list authentication methods enabled
vault auth list
```

Create a the secret path for your applications apps/

```bash
# vault secrets enable -path={your apps} {engine}
vault secrets enable -path=apps kv
# use default engine path, for example kv > /kv
vault secrets enable kv

# list secrets in apps
vault list apps

# remove secret path
vault secrets disable apps
```

Write/Read/Delete secrets in apps/myapp

```bash
# write secrets in apps/
vault kv put apps/myapp key1=value1
vault kv put apps/myapp key2=value2 key3=value3

# read secrets in apps/myapp
vault kv get apps/myapp
vault kv get -field=key1 apps/myapp
vault kv get -format=json apps/myapp | jq -r .data.data.key1

# delete secrets apps/myapp
vault kv delete apps/myapp

```
jq tool: https://stedolan.github.io/jq/

AppRole engine

```bash
# enable approle engine, by default path approle/
vault auth enable approle

# disable approle engine
vault auth disable approle
```

Create a policy file mypolicy.hcl:

>
>path "apps/myapp" {
>
>capabilities = ["read", "list"]
>
>}
>

```bash
vault policy write my-policy mypolicy.hcl
```

Create role myrole and associate policy

```bash
vault write auth/approle/role/myrole secret_id_ttl=120m token_ttl=60m token_max_tll=120m policies="my-policy"
```

Read role-id
```bash
vault read auth/approle/role/myrole/role-id
```
Write secret-id
```bash
vault write -f auth/approle/role/myrole/secret-id
```
Login application with role-id/secret-id
```bash
vault write auth/approle/login role_id=${ROLE_ID} secret_id=${SECRET_ID}
```

VAULT_NAMESPACE

X-Vault-Namespace
x-Vault-token


