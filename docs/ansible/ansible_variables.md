# Variables

Ansible allows you to define variables in over 20 different places. The hierarchy determines which value wins if the same variable occurs multiple times.

!!! tip "The rule of thumb in Ansible is: the more specific the location of a variable, the higher its priority. A value that applies only to a specific task overrides a value that applies to the entire playbook. A value for a specific host (`host_vars`) overrides a value for an entire group (`group_vars`)."

- **Highest priority**: Variables that you type directly into the command line when running the playbook (`-e` or `--extra-vars`)
- **Lowest priority**: The `defaults/main.yml` file in an Ansible role. These values ​​are only fallback options if the variable has not been defined anywhere else.

## Variable precedence

The order of precedence from least to greatest (the last listed variables override all other variables):

1. Command-line values (for example, `-u user`, these are not variables)
2. Role defaults
3. Inventory file or script group vars
4. Inventory `group_vars/all`
5. Playbook `group_vars/all`
6. Inventory `group_vars/*`
7. Playbook `group_vars/*`
8. Inventory file or script host vars
9. Inventory `host_vars/*`
10. Playbook `host_vars/*`
11. Host facts and cached `set_facts`
12. Play vars
13. Play `vars_prompt`
14. Play `vars_files`
15. Role vars
16. Block vars
17. Task vars
18. `include_vars`
19. Registered vars and `set_facts`
20. Role params
21. Inlcude params
22. Extra vars (for example, `-e "user=username"`)

## Best practices

!!! warning "Just because you can define variables anywhere doesn't mean you should."

- Use `group_vars` as standard
- Avoid `--extra-vars`
- Keep playbooks clean
- Roles should stay flexible

## Register variables

You can create a variable from the output of an Ansible task using `register`.

```yaml
tasks:
  - name: Run command and register output
    ansible.builtin.shell: /usr/bin/foo
    register: foo_result
    ignore_errors: true

  - name: Run shell using the registered output
    when: foo_result.rc == 5
    ansible.builtin.shell: /usr/bin/bar
```

## Lists

A list variable combines a variable name with multiple values. You can store the multiple values as an itemized list or in square brackets `[]`, separated with commas.

```yaml
example:
  - test
  - anothertest
  - differenttest
```

To reference on a specific value:

```yaml
region: "{{ region[0] }}"
```

## Dictionaries

A dictionary stores data in key-value pairs. Usually, you use dictionaries to store related data.

```yaml
example:
  test1: one
  test2: two
```

To reference a specific value:

```yaml
example['test1'] # or
example.test1
```

### Combine dictionaries

To merge dictionaries, use `combine`.

```yaml
vars:
  example1:
    name: Tester1
    age: 1

  example2:
    name: Tester2
    age: 2

tasks:
  - name: Combine to dicts
    ansible.builtin.set_fact:
      merged_examples: "{{ example1 | ansible.builtin.combine(example2) }}"
```
