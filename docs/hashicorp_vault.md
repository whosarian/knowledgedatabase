# Hashicorp Vault

Hashicorp Vault is an identity-based system for secret management and protection of sensitive data. It serves as a central instance to save passwords, API-keys and certificates protected and to control and monitor the access on them.

!!! warning "Without a centralized solution secrets are often written in configuration files, variables, repositories, ..."

Hashicorp Vault fixes the problem with replacing static secrets (that are valid infinite) with dynamic secrets, that are generated on demand and expire automatically.

## Core functions

1. **Secure storage**: All data is protected at rest using AES-256-GCM
1. **Dynamic secrets**: Secrets are generated in real time and only exist as long as they are needed
1. **Data encryption**: APIs can encrypt data without granting the application access to the primary key
1. **Leasing & renewal** Every secret is bound to a lease that expires
1. **Revocation**: In case of a security incident, all generated keys can be revoked with only one command

## Important terms

| Term | Description |
| --- | --- |
| Storage Backend | Place where the data is physically stored |
| Auth Methods | Mechanisms with which the users can authenticate |
| Secret Engines | Components that store or generate data |
| Policies | Rules that define who can access which paths |
| Tokens | The primary method to authenticate inside of Vault |

## Sealing mechanism

The main point of Hashicorp Vault is the seal and unseal process. When a Vault server starts, its state is sealed. In this state it can access encrypted data but it cant decrypt it.

To unseal the Vault a defined number of people (threshold) must enter their parts of the key to reconstruct the master key.

## Installation

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo

sudo yum -y install vault
```

### Configuration

```json
storage "file" {
  path = "/opt/vault/data"
}

listener "tcp" {
  address     = "0.0.0.0:8200"
  tls_disable = 1
}

ui = true
```

Make sure the data directory exists and is owned by the user `vault`:

```bash
sudo mkdir -p /opt/vault/data
sudo chown -R vault:vault /opt/vault/data
```

### Start service

```bash
sudo systemctl enable vault
sudo systemctl start vault
```

### Initialize and unseal

After the installation the vault is sealed and uninitialized. That means the storage is protected and there is no master key.

```bash
export VAULT_ADDR='http://127.0.0.1:8200'
vault operator init
```

After that command you get:

1. **Unseal keys**
1. **Initial root token**

!!! danger "Never save those keys on the same server."

Now you can unseal the vault using the following command (usually 3 times) and providing the different keys:

```bash
vault operator unseal
```

After that you can login with the root token:

```bash
vault login <ROOT_TOKEN>
```
