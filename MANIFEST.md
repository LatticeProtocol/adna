---
type: manifest
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [manifest, governance]
---

# lattice-adna — Project Manifest

## Project Identity

**lattice-adna** — A quick-start Obsidian vault for building and managing Lattice YAML definitions with AI agents.

This repo is a self-contained aDNA deployment that anyone can clone and immediately start using. It includes lattice validation tools, canvas interop, example lattices, a full template library, and the R&D→PRD→RFC planning pipeline — everything needed to design, validate, and iterate on lattice definitions.

## Architecture

This project uses the **aDNA (Agentic DNA)** knowledge architecture — a bare triad deployment.

```
lattice-adna/
├── what/    # WHAT — Knowledge objects, context library, lattice tools
├── how/     # HOW — Plans, sessions, templates, pipelines
└── who/     # WHO — People, teams, coordination, governance
```

| Layer | Question | Contains |
|-------|----------|----------|
| **what/** | WHAT does this project know? | Context library, decisions, aDNA docs, lattice YAML tools + schema + examples |
| **how/** | HOW does this project work? | Missions, sessions, 10 templates, backlog, campaigns, skills, PRD/RFC pipeline |
| **who/** | WHO is involved? | People, teams, coordination, governance |

## Entry Points

| Audience | Start Here | Then |
|----------|-----------|------|
| **Agents** | `CLAUDE.md` (auto-loaded) | `STATE.md` → `how/sessions/active/` → work |
| **Humans** | `README.md` | `MANIFEST.md` → browse triad → `STATE.md` |

## Key Components

### Lattice YAML Tools

| Tool | Location | Purpose |
|------|----------|---------|
| `lattice_validate.py` | `what/lattices/tools/` | Validate `.lattice.yaml` against JSON Schema |
| `lattice2canvas.py` | `what/lattices/tools/` | Convert lattice YAML → Obsidian canvas |
| `canvas2lattice.py` | `what/lattices/tools/` | Convert Obsidian canvas → lattice YAML |
| `lattice_yaml_schema.json` | `what/lattices/` | JSON Schema for lattice definitions |

### Planning Pipeline

```
how/pipelines/prd_rfc/
├── 01_research/      → Problem space exploration
├── 02_requirements/  → PRD authoring (human gate)
├── 03_design/        → RFC authoring
└── 04_review/        → Final approval (human gate)
```

### Templates (10)

| Template | Auto-triggers in |
|----------|-----------------|
| `template_session.md` | `how/sessions/active/` |
| `template_mission.md` | `how/missions/` |
| `template_context.md` | `what/context/` |
| `template_adr.md` | `what/decisions/` |
| `template_coordination.md` | `who/coordination/` |
| `template_backlog.md` | `how/backlog/` |
| `template_campaign.md` | `how/campaigns/` |
| `template_prd.md` | `how/pipelines/prd_rfc/02_requirements/` |
| `template_rfc.md` | `how/pipelines/prd_rfc/03_design/` |
| `template_skill.md` | `how/skills/` |

## Active Builds

| Plan | Status | Description |
|------|--------|-------------|
| *(none yet)* | | |

See `how/missions/` for mission details and `STATE.md` for current operational state.
