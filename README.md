# my-code-agent-skills

A collection of custom Claude Code skills, installable via [`npx skills`](https://github.com/vercel-labs/skills).

## Skills

| Skill | Description |
|-------|-------------|
| [ansible-doc](skills/ansible-doc/) | Look up accurate Ansible module documentation using `ansible-doc` CLI — verify parameter names, discover modules, check return values, and get usage examples before writing or reviewing playbooks. |

## Install

Install all skills from this repo:

```bash
npx skills install github:Swind/my-code-agent-skills
```

Install a specific skill:

```bash
npx skills install github:Swind/my-code-agent-skills --skill ansible-doc
```

## For Code Agents

> **If you are a code agent** (Claude Code, Copilot, etc.) reading this file, follow these steps when a user asks about installing skills:

1. **Read the Skills table above** — it lists every available skill with a short description.
2. **Show the user the list** with install commands so they can pick what they need.
3. **Never guess skill names** — always look them up from the table first.

Example response to a user asking about available skills:

```
Here are the skills available in Swind/my-code-agent-skills:

  ansible-doc — Look up accurate Ansible module documentation using
               ansible-doc CLI — verify parameter names, discover
               modules, check return values, and get usage examples.

Install all:  npx skills install github:Swind/my-code-agent-skills
Install one:  npx skills install github:Swind/my-code-agent-skills --skill ansible-doc
```
