---
type: skill
skill_type: agent
created: 2026-03-23
updated: 2026-04-03
status: active
category: onboarding
trigger: "Root CLAUDE.md project creation flow — user wants to create a new project"
last_edited_by: agent_init
tags: [skill, project, fork, onboarding, lattice]

requirements:
  tools: []
  context: ["CLAUDE.md", "MANIFEST.md"]
  permissions: ["copy directories", "write files in workspace directory", "remove .git and .obsidian from fork"]
---

# Skill: Project Fork

## Overview

Creates a new aDNA project by forking the `.adna/` base template. The fork receives the full aDNA structure (triad, templates, skills, context library, lattice tools) as a new project with its own git repository. The forked project's `MANIFEST.md` is prepared for first-run onboarding.

This skill is called from the **root CLAUDE.md** at `~/lattice/CLAUDE.md` when a user wants to create a new project.

## Trigger

Invoked by the root CLAUDE.md project creation flow. Not triggered automatically — always called from the root governance.

## Parameters

| Parameter | Source | Required |
|-----------|--------|----------|
| `carry_forward_answers` | Any project name/description already collected from the calling flow | No |

The workspace root is always the directory containing this `.adna/` template (detected automatically). The template source is always `.adna/`.

## Requirements

### Tools/APIs
- File copy (`cp -r`)
- File deletion (`rm -rf .obsidian/plugins/ .obsidian/themes/`)
- Git init (`git init`)
- File read/write (MANIFEST.md frontmatter editing)

### Context Files
- `.adna/MANIFEST.md` — to verify `role: template` in the source and strip it in the fork

### Permissions
- Write to the workspace root directory (same level as `.adna/`)
- Copy the `.adna/` directory structure

## Implementation

### Step 1: Collect Project Identity

If `carry_forward_answers` are provided (from the calling flow), use them. Otherwise ask:

1. **Project name** — base folder name (the `.aDNA` suffix is appended automatically). Must be lowercase with underscores. Example: `my_research_lab`, `acme_crm`, `sleep_study` → creates `my_research_lab.aDNA/`
2. **Brief description** — 1-2 sentences describing the project's purpose

Validate the project name:
- Lowercase letters, digits, and underscores only
- Must not collide with an existing directory in the workspace
- Must not be `.adna` (that's the base template)
- Must not be `latlab` or `lattice-protocol` (infrastructure repos)

### Step 2: Confirm Target Location

The target directory is `<workspace_root>/<project_name>.aDNA/` (the `.aDNA` suffix marks it as an aDNA project — see Standard §3.5).

Report to the user:
> "I'll create your project at `<project_name>.aDNA/`. This will fork the full aDNA structure — triad directories, templates, skills, context library, and lattice tools. The base template at `.adna/` stays untouched."

If the user explicitly requests no suffix, respect their preference.

Ask for confirmation before proceeding.

### Step 3: Fork the Template

```bash
cp -r .adna/ <project_name>.aDNA/
cd <project_name>.aDNA/
git init

# Preserve portable Obsidian config (settings, appearance, snippets)
# but remove plugin binaries (15MB+) — user runs setup.sh to install them
rm -rf .obsidian/plugins/ .obsidian/themes/
rm -f .obsidian/workspace.json .obsidian/graph.json
```

Note: `.adna/` has no `.git/` directory (it's inside the parent repo), so no git cleanup needed.

This gives the new project:
- The full `who/what/how/` triad structure
- All templates, skills, context library, and lattice tools
- A fresh git repository with no history
- Portable Obsidian config (app settings, appearance, CSS snippets, hotkeys, plugin list)
- Run `./setup.sh` to download plugins and theme (~15MB, requires network)

### Step 4: Prepare for Onboarding

Edit the forked project's governance files to set up first-run detection:

**MANIFEST.md:**
- Remove `role: template` from frontmatter (or delete the field entirely)
- Set `last_edited_by: agent_init`
- Set `updated: <today's date>`
- If the user provided a project description in Step 1, update the project description section

**STATE.md:**
- Set `last_edited_by: agent_init`
- Set `updated: <today's date>`

**CLAUDE.md:**
- Set `last_edited_by: agent_init` in frontmatter
- Set `updated: <today's date>` in frontmatter

These markers ensure the project's CLAUDE.md first-run detection will trigger `skill_onboarding.md` on next open.

### Step 5: Offer Immediate Onboarding

Ask the user:
> "Your project is ready at `<path>`. Would you like to run the onboarding interview now to customize it for your domain? Or you can open it later — the setup will trigger automatically on first run."

**If now:**
- Instruct the user to open a new Claude Code session in the project directory: `cd <project_path>` then run `claude`
- Carry forward any answers from Step 1 so the user doesn't repeat themselves
- Note: the onboarding will trigger automatically from the project's own CLAUDE.md first-run detection

**If later:**
- Report the path and explain that onboarding triggers automatically on first `claude` invocation inside the project directory
- Suggest: "To start working in your project, run `cd <project_path> && claude`"

### Step 6: Report

Confirm to the user:
- **Created**: `<workspace_root>/<project_name>/`
- **Structure**: Full aDNA triad (who/what/how) + templates + skills + context library
- **Git**: Initialized with fresh repository (no history from template)
- **Next**: Open the project directory to begin onboarding

## Outputs

| Output | Type | Description |
|--------|------|-------------|
| Project directory | Directory | Full aDNA structure at `<workspace_root>/<project_name>/` |
| Prepared MANIFEST.md | File | `role: template` removed, `agent_init` marker set |
| Prepared STATE.md | File | `agent_init` marker set |
| Prepared CLAUDE.md | File | `agent_init` marker set |
| Fresh git repo | Git | `git init` with no history |

## Error Handling

| Error | Cause | Resolution |
|-------|-------|------------|
| Directory already exists | Name collision | Warn user, ask for different name |
| Workspace not writable | Permissions issue | Suggest creating the directory manually or choosing a different location |
| Copy fails | Disk space or permissions | Report the error with the specific path that failed |
| .adna/ doesn't have `role: template` | Not the canonical template | Warn the user — the base template may be corrupted. Suggest `git pull` |

## Related

- [[how/skills/skill_onboarding|skill_onboarding.md]] — Runs after fork to customize the new project
- [[what/docs/projects_folder_pattern|projects_folder_pattern.md]] — Workspace architecture documentation
