---
type: directory_index
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [directory_index, root]
---

# lattice-adna — Agent Guide

## Purpose

This is the root agent reference for the lattice-adna project. It provides structured navigation for AI agents operating in this vault.

## Quick Orientation

| Document | What You'll Find |
|----------|-----------------|
| **CLAUDE.md** | Agent operating context — safety rules, startup checklist, protocols |
| **MANIFEST.md** | Project overview — architecture, entry points, active builds |
| **STATE.md** | Current operational state — phase, blockers, next steps |
| **README.md** | Human-readable getting-started guide |

## Project Structure

```
lattice-adna/
├── what/    # WHAT — Knowledge objects, context library, decisions
├── how/     # HOW — Plans, sessions, templates
└── who/     # WHO — People, teams, coordination, governance
```

## Agent Startup

1. **CLAUDE.md** — auto-loaded; confirms project structure and rules
2. **First-run check** — if uncustomized vault, invoke onboarding skill (`how/skills/skill_onboarding.md`)
3. **STATE.md** — operational snapshot: current phase, blockers, next steps
4. **`how/sessions/active/`** — check for conflicting sessions
5. **`who/coordination/`** — read any urgent cross-agent notes
6. **Create session file** in `how/sessions/active/` and begin work

## Layer References

- [what/AGENTS](what/AGENTS.md) — Knowledge objects (WHAT)
- [how/AGENTS](how/AGENTS.md) — Operations (HOW)
- [who/AGENTS](who/AGENTS.md) — Organization (WHO)

## Safety Rules

- **Read before write** — always read current content before modifying
- **Check `updated` field** — if updated today by someone else, confirm before overwriting
- **Set `last_edited_by` and `updated`** — on every modification
- **New files are safe** — creating new files has no collision risk

## Priority Hierarchy

1. **Data integrity** — never corrupt or lose existing data
2. **User-requested tasks** — explicit instructions from current user
3. **Operational maintenance** — session tracking, plan updates
4. **Exploration** — research, audits, improvements
