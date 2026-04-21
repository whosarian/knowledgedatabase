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

## Sealing mechanism

The main point of Hashicorp Vault is the seal and unseal process. When a Vault server starts, its state is sealed. In this state it can access encrypted data but it cant decrypt it.

To unseal the Vault a defined number of people (threshold) must enter their parts of the key to reconstruct the master key.
