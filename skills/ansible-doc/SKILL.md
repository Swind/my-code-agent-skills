---
name: ansible-doc
description: Use ansible-doc CLI to look up accurate Ansible module documentation before writing or reviewing playbooks. Trigger this skill whenever writing, editing, or reviewing Ansible playbooks or roles — especially when using any module you're not 100% certain about. Use it to verify correct parameter names, discover available modules for a task, check required vs optional parameters, and get real usage examples. Always consult ansible-doc before using less-common modules like community.general.*, ansible.posix.*, cloud provider modules, or any module where a parameter name is unclear. Don't guess module parameters — look them up.
---

# Ansible Doc Skill

When writing Ansible playbooks, incorrect module parameters are a common failure point — especially for modules outside `ansible.builtin`. This skill ensures you always use accurate, up-to-date parameter names and values by querying the local `ansible-doc` CLI.

## When to use ansible-doc

- Before writing any task that uses a module you haven't used recently
- When you're unsure about a parameter name (e.g., is it `dest` or `path`? `state: present` or `state: installed`?)
- When the user asks what a module does or what options it supports
- When searching for the right module to accomplish a task
- When checking what a module returns (for `register:` usage)

## Commands

### Look up a module (quick parameter reference)

```bash
ansible-doc -s <module_name>
```

This returns a snippet — a concise YAML template showing all parameters with inline descriptions. Use this first; it's fast and fits in context.

**Example:**
```bash
ansible-doc -s ansible.builtin.user
ansible-doc -s community.general.npm
ansible-doc -s amazon.aws.ec2_instance
```

### Look up a module (full docs + examples)

```bash
ansible-doc <module_name>
```

Use this when you need:
- Detailed explanations of parameter behavior
- Real usage examples
- Notes and caveats

### Look up a module (JSON — best for return values)

```bash
ansible-doc -j <module_name>
```

Parse the output to get structured data. Especially useful when checking what a module returns after `register:`:

```bash
ansible-doc -j ansible.builtin.stat | python3 -c "
import sys, json
d = json.load(sys.stdin)
m = list(d.values())[0]
print('RETURN VALUES:')
for k, v in m.get('return', {}).items():
    print(f'  {k}: {v.get(\"description\", \"\")[:80]}')
"
```

### Search for a module by keyword

```bash
ansible-doc -l | grep -i <keyword>
```

Use this when you're not sure which module to use for a task.

**Example:**
```bash
ansible-doc -l | grep -i "docker"
ansible-doc -l | grep -i "firewall"
ansible-doc -l | grep -i "systemd"
```

### List modules by plugin type

```bash
ansible-doc -l -t <type>
```

Types: `module` (default), `lookup`, `filter`, `callback`, `connection`, `inventory`

## Workflow when writing a playbook task

1. **Identify the module** — if unsure which module to use, search first: `ansible-doc -l | grep -i <keyword>`
2. **Check parameters** — run `ansible-doc -s <module>` to see exact parameter names and types
3. **Verify required params** — required parameters are marked in the snippet output
4. **Check examples if needed** — run `ansible-doc <module>` for real usage examples
5. **Write the task** — use exact parameter names from the documentation

## Module naming

Always use the fully qualified collection name (FQCN) when possible:
- `ansible.builtin.copy` not `copy`
- `community.general.npm` not `npm`
- `ansible.posix.firewalld` not `firewalld`

If unsure of the full name, search: `ansible-doc -l | grep <short_name>`

## Common module families

| Prefix | Scope |
|--------|-------|
| `ansible.builtin.*` | Core modules — always available |
| `ansible.posix.*` | POSIX/Linux — firewalld, sysctl, mount, etc. |
| `community.general.*` | Large community collection — npm, docker, etc. |
| `community.docker.*` | Docker-specific modules |
| `amazon.aws.*` | AWS cloud modules |
| `google.cloud.*` | GCP cloud modules |
| `kubernetes.core.*` | Kubernetes / kubectl modules |
