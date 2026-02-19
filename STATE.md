---
type: state
created: 2026-02-17
updated: 2026-02-17
status: active
last_edited_by: agent_init
tags: [state, governance]
---

# Operational State

Dynamic operational snapshot for cold-start orientation. Updated each session.

## Current Phase

**Phase 0 — Skeleton Deployed.** The lattice-adna quick-start repo is initialized with the full aDNA triad, lattice YAML tools, 9 templates, PRD/RFC pipeline, and 3 example lattices. Ready for first use.

## Recent Decisions

| Date | Decision | Source |
|------|----------|--------|
| 2026-02-17 | Initial repo created with aDNA bare triad + lattice tools | M8 Objective 2 |

## Active Blockers

None.

## What's Working

- aDNA skeleton deployed (what/how/who triad, governance files, 9 templates)
- Lattice YAML validation tool (`lattice_validate.py`)
- Canvas-YAML bidirectional conversion (`lattice2canvas.py`, `canvas2lattice.py`)
- JSON Schema for `.lattice.yaml` format
- 3 example lattice files (deep research, orchestrator, protein binder)
- R&D→PRD→RFC planning pipeline with 4 stages
- Session tracking ready (`how/sessions/active/`)
- Mission system ready (`how/missions/`)
- Campaign system ready (`how/campaigns/`)
- Backlog system ready (`how/backlog/`)
- Context library ready (`what/context/`)
- 9 CSS snippets for Obsidian visual polish

## Next Steps

1. **Run `./setup.sh`** — installs 12 community plugins and Tokyo Night theme
2. **Install Space Grotesk font** (optional) — download from [Google Fonts](https://fonts.google.com/specimen/Space+Grotesk)
3. **Create your first lattice** — copy an example from `what/lattices/examples/` and customize
4. **Validate your lattice** — use `lattice_validate.py` to check against schema
5. **Start a session** — create a session file in `how/sessions/active/` and begin work
