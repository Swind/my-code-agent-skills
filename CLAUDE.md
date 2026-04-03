# Claude Code Skills Repository

This repository stores custom Claude Code skills that can be installed into other projects via `npx skills`.

## Repository Structure

```
my-code-agent-skills/
├── CLAUDE.md               # This file — architecture and conventions for Claude
├── README.md
├── skills-lock.json        # Tracks externally installed skills (do not edit manually)
├── skills/                 # Custom skills published via this repo
│   └── <skill-name>/
│       ├── SKILL.md        # Required — skill entrypoint (name + description frontmatter)
│       └── ...             # Optional scripts, examples, assets
└── .claude/
    └── skills/             # Locally installed skills (for use within this repo)
        └── skill-creator/  # Tool for Claude to create new skills — NOT published
```

## Key Conventions

- All publishable skills live under `skills/`. Each skill gets its own subdirectory.
- Every skill directory must contain a `SKILL.md` with at minimum:
  ```yaml
  ---
  name: skill-name
  description: ...
  ---
  ```
- `.claude/skills/` is for tools used **within this repo only** and is not distributed.
- `skill-creator` (in `.claude/skills/`) is a tool for Claude to use when building new skills. It is never copied to `skills/`.

## Creating a New Skill

Use the `skill-creator` skill (already installed in `.claude/skills/`). It will guide through:
1. Capturing intent and requirements
2. Writing the `SKILL.md`
3. Running evaluations and iterating

New skills are placed in `skills/<skill-name>/` when complete.

## Installing Skills from This Repo

Other projects can install skills from this repo using:

```bash
npx skills install github:<username>/my-code-agent-skills
```

`npx skills` automatically discovers skills in the `skills/` directory by scanning for `SKILL.md` files.
