# Inventory

The inventory is the main part of the Ansible infrastructure. Without the inventory Ansible would not know which hosts it has to manage.  
It is a file, that contains a list of the managed nodes. The nodes can be organzied in groups to orchestrate tasks on specific hosts.

## Static inventories

```yaml
all:
  children:
    webservers:
      hosts:
        web01.example.com:
        web02.example.com:
    dbservers:
      hosts:
        db01.internal:
          ansible_host: 192.168.1.50
```

## Inventory variables

Variables can be defined directly in the inventory to set host-specific options:

- **Host variables**: Valid for specific hosts
- **Group variables**: Valid for all hosts of a group

!!! tip "In bigger projects you should not write every variable directly into the inventory but in the directories `host_vars/` and `group_vars/`. Ansible automatically searches these folders for files with the same name as hosts or groups."

## Parameter

- `ansible_host`: The actual IP or DNS, if the name is only an alias
- `ansible_user`: The username for the SSH login
- `ansible_ssh_private_key_file`: Path to a specific SSH key
- `ansible_python_interpreter`: Path to Python on a managed host

## Dynamic inventories

In cloud environments IP's change constantly. A static inventory would be outdated very fast.

!!! tip "Inventory Plugins: Ansible uses plugins to request the cloud API to generate the hostlist in realtime"

## Test inventories

Before you execute a playbook you can check if Ansible see's all hosts correctly:

```bash
ansible-inventory -i hosts.yml --list # list all hosts

ansible all -i hosts.yml -m ping # checks the availability of all hosts
```
