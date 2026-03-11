# Writing Ansible Extensions

When you reach the point where complex Jinja2 chains become unreadable, or you need to control proprietary company software for which there are no official modules, it's time to extend Ansible with Python.

## Modules vs. Plugins

| | Modules | Plugins |
| --- | --- | --- |
| Place of execution | Managed node | Control node |
| Process | Ansible copies the Python script to the target server via SSH, executes it there, and then deletes it | Runs locally. Transforms data or retrieves external information before or during tasks are sent to the target server |
| Purpose | Change system states (install packages, call APIs, create files) | Manipulating, formatting, or importing data from external sources (e.g., password managers) |

## Filter plugins

Sometimes Jinja2-filter are not enough to restructure complex data structures.  
Create a folder named `filter_plugins/` on the root directory of your playbook or role.  

Example: A plugin that reverts the text.

```python
#!/usr/bin/python

class FilterModule(object):
  def filters(self):
    return {
      'reverse_string': self.reverse_string
    }
  
  def reverse_string(self, text):
    if not isinstance(text, str):
      return text
    return text [::-1]
```

Usage in playbook:

```yaml
- name: Test filter
  ansible.builtin.debug:
    msg: "{{ 'Hello World' | reverse_string }}"
```

## Modules

If you want to access your own REST API or control an obscure command-line tool, you write a module.  
Create a folder named `library/` in the root directory of your playbook or role.  

Example:

```python
#!/usr/bin/python
from ansible.module_utils.basic import AnsibleModule

def runModule():
  module_args = dict(
    name=dict(type='str', required=True),
    state=dict(type='str', choices=['present', 'absent'], default='present')
  )

  module = AnsibleModule(
    argument_spec=module_args,
    supports_check_mode=True
  )

  result = dict(
    changed=False,
    message=''
  )

  if module.params['state'] == 'present':
    result['changed'] = True
    result['message'] = f"The element {module.params['name']} has been created"
  else:
    result['changed'] = False
    result['message'] = "Nothing to do"

  module.exit_json(**result)

def main():
  run_module()

if __name__ == '__main__':
  main()
```

Usage in playbook:

```yaml
- name: Execute own module
  my_custom_module:
    name: "Database"
    state: "present"
  register: result
```

## Understand return values

If you write a module, youre in charge to tell Ansible what happened. Always use `module.exit_json()` on success and `module.fail_json()` on error.  

The most important keys in the `result`-dictionary are:

- `changed`: Absolutely essential! Only if this is true will Ansible display a yellow "changed" in the terminal. If it's false, a green "ok" will appear.
- `failed`: The playbook terminates in red (often automatically set by `fail_json`).
- `message`: A human-readable explanation, what has been done and why it didnt work.

!!! tip "Only write custom code when absolutely necessary. Before writing a Python module that calls an API, check if the built-in `ansible.builtin.uri` module can already solve the task (idempotently)."
