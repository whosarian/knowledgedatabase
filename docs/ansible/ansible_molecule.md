# Molecule

Molecule is the official testing framework for Ansible. It answers the most critical question of all: "Does my role actually do what it's supposed to, and on all supported operating systems?"

Instead of manually testing playbooks on a VM, Molecule automates the entire process:

1. Creates temporary test environments
2. It applies your Ansible role to this environment
3. It checks whether the role is idempotent (whether a second pass is error-free and without changes)
4. It verifies the final state
5. It ultimately destroys the test environment again

## Test lifecycle

When you start Molecule, it goes through a fixed chain of steps:

1. **Dependency**: Downloads other roles or collections that your role requires (from `requirements.yml`)
2. **Lint**: Executes `ansible-lint` and `yamllint` to check your code for syntax and style errors
3. **Create**: Start up the test-infrastructure
4. **Converge**: Executes your role
5. **Idempotence**: The role will execute a second time and fail if tasks report the status as changed again
6. **Verify**: Runs tests to check if the web server is actually running or if the port is open
7. **Destroy**: Deletes the containers and cleans up

## Directory structure

When you initialise a role with Molecule

```sh
molecule init role my_role -d docker
```

Ansible creates the directory `molecule/default/` inside of your role, including:

- `molecule.yml`: Main configuration
- `converge.yml`: Minimal playbook that does nothing but call the role
- `verify.yml`: Where the actual test are written

## Verfiy playbook

The best practice way to test is directly with Ansible modules (like `ansible.builtin.assert`) in the `verify.yml`.  

Example: Test if Nginx service has started and port 80 is opened

```yaml
---
- name: Verify
  hosts: all
  tasks:
    - name: Get information
      ansible.builtin.service_facts:
        that:
          - ansible_facts.services['nginx.service'].state == 'running'
          - ansible_facts.services['nginx.service'].state == 'enabled'
        fail_msg: "Nginx is not running or not activated"

    - name: Check if port 80 is open
      ansible.builtin.wait_for:
        port: 80
        timeout: 5
```

## Commands

```bash
molecule test # full run from create to destroy
molecule converge # starts up the containers but doesnt delete them
molecule login # opens a SSH connection to a running test container
molecule destory # destroys containers that are running after converge
```
