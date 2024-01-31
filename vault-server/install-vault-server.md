```bash
___    __             __________ 
__ |  / /_____ ____  ____  /_  /_
__ | / /_  __ `/  / / /_  /_  __/
__ |/ / / /_/ // /_/ /_  / / /_  
_____/  \__,_/ \__,_/ /_/  \__/  
                                 
Secure, store, and tightly control access to tokens, passwords, certificates, and encryption keys for protecting secrets
and other sensitive data using a UI, CLI, or HTTP API.
```

---
#### 1. Update the vault section in the `docker-compose` file
```bash
version: '3.9'

services:
  vault-server:
    image: vault:1.13.3
    container_name: vault
    environment:
      VAULT_ADDR: http://127.0.0.1:8200
    platform: linux/amd64
    ports:
      - "8200:8200"
    volumes:
      - vault:/vault/file:rw
      - ./vault:/vault/config:rw
    cap_add:
      - IPC_LOCK
    entrypoint: vault server -config=/vault/config/vault.json
networks:
  default:
    name: api-network
volumes:
  vault:  
```

#### 2. Create a folder named `vault` and add a file named `vault.json`
```bash
{
  "backend": {
    "file": {
      "path": "/vault/file"
    }
  },
  "listener": {
    "tcp": {
      "address": "0.0.0.0:8200",
      "tls_disable": 1
    }
  },
  "ui": true
}
```

#### 3. Run docker command to start the stack:
```bash
$ docker-compose up -d vault-server
```

#### 4. Run the following command to initialize the vault instance.
```bash
$ docker exec -it vault-server vault operator init -n 1 -t 1
Unseal Key 1: inlYFIDUaZ6VCIvw+arg4HdrCLlibOf4eUHOJSQCFKA=

Initial Root Token: hvs.R7sCTOKns24kCknjCZa6dT4R
...
```

#### 5. Run the unseal command to decrypt the contents of the vault. Note: this step has to be performed each time the vault container is restarted.
```bash
$ docker exec -it vault-server vault operator unseal [unseal key 1]

-- example --
docker exec -it vault-server vault operator unseal inlYFIDUaZ6VCIvw+arg4HdrCLlibOf4eUHOJSQCFKA=
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.13.3
Build Date      2023-06-06T18:12:37Z
Storage Type    file
Cluster Name    vault-cluster-a7e7a59e
Cluster ID      abaedb33-c735-345c-65af-ae0d60070856
HA Enabled      false
```

#### 6. Login to vault with the following command using the initial root token:
```bash
$ docker exec -it vault-server vault login [Initial Root Token]

-- example --
docker exec -it vault-server vault login hvs.R7sCTOKns24kCknjCZa6dT4R
```

#### 7. Enable the kv secrets engine
```bash
$ docker exec -it vault-server vault secrets enable -version=2 kv
```

#### 8. Update file `.env`
- Update vault tokens used in each service
    ```bash
    RPS_VAULT_TOKEN=[initial root token]
    MPS_VAULT_TOKEN=[initial root token]
    
    -- example --
    RPS_VAULT_TOKEN=hvs.R7sCTOKns24kCknjCZa6dT4R
    MPS_VAULT_TOKEN=hvs.R7sCTOKns24kCknjCZa6dT4R
    ```

- update secrets path
    ```bash
    MPS_SECRETS_PATH=kv/data/
    RPS_SECRETS_PATH=kv/data/
    ```

#### 9. Press ctrl-c to stop the stack 
#### 10. Restart the stack using the command docker-compose up.
#### 11. Rerun command in step 5 to unseal the vault.

---
Reference sources: 
    - [Use Docker and Vault in Production Mode](https://open-amt-cloud-toolkit.github.io/docs/2.0/Docker/dockerLocal_prodVault/)
