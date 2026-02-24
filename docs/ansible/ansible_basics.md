# Ansible Basics

Ansible is an open source tool that is used to automate configurationmanagement, software provision and orchestration.  
There are 2 important concepts:

- **Agentless**: The machines orchestrated by Ansible don't require any software. Ansible communicates via SSH (Linux) or WinRM (Windows).
- **Idempotence**: You can execute the same operation multiple times without changing the outcome.

## Components

- **Control Node**: The machine that has Ansible installed
- **Managed Nodes**: The machines to be managed
- **Inventory**: A list of all managed machines
- **Modules**: The "tools" to perform actions on your machines

## Playbooks

A playbook contains a set of tasks to be performed on the managed nodes.  

!!! tip "A task always consists of exactly one module call."
