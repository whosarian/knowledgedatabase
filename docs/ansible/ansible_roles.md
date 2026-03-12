# Roles

A role in Ansible is a self-contained, reusable package consisting of tasks, variables, files and templates.  
Roles provide the following advantages:

- Reusability
- Shareability
- Tidiness

## Directory structure

Ansible expects a specific directory structure. When using a role, Ansible automatically looks for a `main.yml` file in these folders.

```sh
roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case
```

!!! tip "This structure can be automatically created using `ansible-galaxy`."

```bash
ansible-galaxy role init new_role
```

## Calling roles

There are mainly three ways to call a role in your playbook.

### 1. The playbook only consists of roles

```yaml
---
- name: Build webserver
  hosts: webservers
  become: yes

  roles:
    - common
    - webserver
    - firewall
```

### 2. Static call inside a task (`import_role`)

Using `import_role` the role is fully read and checked when the playbook starts.

!!! tip "Advantages:"
    - **Error detection**: Syntax errors are detected when starting the playbook
    - **Visibility**: When using the command `ansible-playbook site.yml --list-tasks` you see every task inside an imported role
    - **Tags**: A tag on the import task provides the tag to every task of the role

!!! danger "Disadvantages:"
    - Loops (`loop`, `with_items`) and `import_role` are not compatible. That simply doesn't work, because Ansible doesn't know the variables when reading then in before starting

Example:

```yaml
tasks:
  - name: Static role call
    ansible.builtin.import_role:
      name: static_role
```

### 3. Dynamic call inside a task (`include_role`)

Using `include_role`, Ansible doesn't know what's in that role when the playbook starts. It only finds out when the task is executed.

!!! tip "Advantages:"
    - **Loops**: You can call a role multiple times with different variables
    - **Dynamic variables**: You can use variables that were created in a task immediately before it

!!! danger "Disadvantages:"
    - **Hidden errors**: A typo in the role only becomes apparent when Ansible reaches that exact point. If the playbook spends an hour installing servers beforehand, it only aborts at the very end
    - **Invisible**: Using `--list-tasks` only the `include_role`-task is displayed, not the tasks inside

Example:

```yaml
tasks:
  - name: Dynamic role call
    ansible.builtin.include_role:
      name: dynamic_role
```

### When use what?

!!! tip "Always use `import_role` by default, as it's more predictable and displays errors immediately. Only switch to `include_role` if you need to execute the role in a loop or if it depends on conditions/variables that are generated during playbook runtime."
