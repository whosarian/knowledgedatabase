# Execution Environments

Execution Environments (EE's) are container-images that include everything you need to run your Ansible playbooks.  
Instead of installing Ansible and dozens of packages on a server you can just start the container that executes your playbook.

## Anatomy

A EE-container consists of several solid layers that interlock:

1. **OS**: A minimal Linux
2. **Software**: Ansible Core and Ansible Runner
3. **Collections**: Extra modules
4. **Python packages**: Python libraries that are needed for modules
5. **System packages**: OS-tools (git, curl, ...)

## Builder and Navigator

- **`ansible-builder`**: Tool that reads your configuration and builds a container image out of it
- **`ansible-navigator`**: Terminal interface (replaces `ansible-playbook`)

## Build an EE

!!! note "To build your own EE you need a file named `execution-environment.yml`."

```yaml
version: 3

dependencies:
  galaxy: requirements.yml
  python: requirements.txt
  system: bindep.txt

images:
  base_image:
    name: quay.io/ansible/ansible-runner:latest
```

Using `ansible-builder build -t ansible_container` the image is build on your local maschine.
