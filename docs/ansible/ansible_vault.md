# Vault

With Ansible vault you can encrypt sensitive data like secrets or certificates. Those vaults use AES256.

## Basic commands

```bash
ansible-vault create secret.yml # Creates new encrypted file
ansible-vault edit secret.yml # Opens an existing vault file
ansible-vault view secret.yml # Displays the content in terminal
ansible-vault encrypt file.yml # Encrypts an existing file
ansible-vault decrypt file.yml # Decrypts an existing file
ansible-vault rekey secret.yml # Changes the password
```

## Execute playbooks with vault

If the playbook uses encrypted variables you have to tell Ansible the password when running it.  

### Method A: Manual input

Ansible prompts the password before the playbook starts

```bash
ansible-playbook site.yml --ask-vault-pass
```

### Method B: Vault file

The password is located in a specific file

```bash
ansible-playbook site.yml --vault-password-file .vault_pass.txt
```

!!! danger "The vault file has to be in the `.gitignore` file!"

## Encrypt variables

```bash
ansible-vault encrypt_string 'secretPassword123' --name 'example_password'
```

The output can be copied in a `group_vars` file:

```yaml
user: admin
password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          38396162626134393931653538663032343934643033323062323064616238633761353232653335
          3531633534643036653232343431353632363539353933390a363765353564613233346538356133
```
