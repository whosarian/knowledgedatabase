# Ansible Lint

Ansible lint is a command line tool to analyze the quality of your code. It reviews your playbooks, roles and collection for:

- Syntax errors
- Best practicses
- Anti patterns
- YAML validation

## Installation and commands

```bash
pip install ansible-lint
```

- `ansible-lint playbook.yml`: Reviews a specific playbook
- `ansible-lint .`: Reviews the whole directory
- `ansible-lint --list-rules`: Displays all rules the tool uses
- `ansible-lint --fix`: Tries to fix simple errors automatically

## Configuration

You can controll the behavior of Ansible lint in a file named `.ansible-lint`. With that you can ignore specific rules.  
Example:

```yaml
exclude_paths:
  - ./old_backups/
skip_list:
  - 'yaml[line-length]'  # ignore line length
  - 'experimental'       # ignore experimental rules
```

!!! tip "Ansible lint integrates seamlessly with VS Code via the Ansible extension or can be set as a pre-commit hook. This allows your code to be checked automatically when saving or committing to Git, before any errors even reach the server."
