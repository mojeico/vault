Vault 

Main config -  /etc/vault.d/vault.hcl


VAULT_ADDR=http://127.0.0.1:8200
export VAULT_ADDR



vault status

vault operator init  - init vault and get keys 








-------------- Create, delete, add, reader secret  - key/value  ----------------


vault secrets enable kv -path=secrets-mojeico-kv


vault kv put secrets-mojeico-kv/secret-name key=value test2=val2 username=my_usernape password=my_password

vault kv get secrets-mojeico-kv/secret-name

vault kv delete secrets-mojeico-kv/secret-name



-------------- Get token   ----------------




- vault token create - generate token for login 



-------------- ENABLE USER PASSWORD ----------------

- vault auth enable userpass - enable login with user and pass 
- vault write auth/userpass/users/my_user password=123password
- vault login -method=userpass username=my_user password=123password

-------------- ENABLE APPROLE  ----------------

- vault auth enable approle - enable login with user and pass


vault write auth/approle/role/gleb \
secret_id_ttl=10m \
token_num_uses=10 \
token_ttl=20m \
token_max_ttl=30m \
secret_id_num_uses=40

vault read auth/approle/role/gleb/role-id

vault write -f auth/approle/role/gleb/secret-id

vault write auth/approle/login role_id=3e98e0c7-c0e4-adec-2a97-9528d762360e secret_id=1d2bcfb5-5e13-3109-1c8d-750f3f3cc2fb

-------------- ALC POLICY ----------------


- vault policy list



- vault secrets enable -path=secret/ kv 

- vim policy.hcl

  path "secret/data/training_data" {            - (secrets/data/path)
      capabilities = ["create", "read"]
  }

- vault policy write firstpolicy policy.hcl


- vault policy read firstpolicy

- vault token create -policy='firstpolicy'       - login with policy 


- WORK - vault kv put secret/data/training_data  key1=value1
- NOT  - vault kv put secret/test_data  key1=value1




vault write  auth/userpass/users/my_user/policies policies="firstpolicy"

--------------- REST API -------------





