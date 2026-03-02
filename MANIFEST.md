---
type: manifest
created: 2026-02-17
updated: 2026-03-02
last_edited_by: agent_init
tags: [manifest, governance]
---

# adna — Project Manifest

## Project Identity

<!-- CUSTOMIZE THIS: Replace the description below with your project's name and purpose. -->
<!-- Example: "**my-project** — AI-powered market research platform for B2B SaaS companies." -->

**adna** — A standalone knowledge architecture for building personal lattices and knowledge graphs. Designed for both humans (via Obsidian) and AI agents (via Claude Code). aDNA is the foundational building block of the Lattice federated computing protocol.

This repo is a self-contained aDNA deployment that anyone can clone and immediately start using. It includes templates, tools, example lattices, and the operational infrastructure (sessions, missions, campaigns) needed to run an AI-native project from day one.

## Architecture

This project uses the **aDNA (Agentic DNA)** knowledge architecture — a bare triad deployment.

```
adna/
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

### Context Library

| Topic | Subtopics | Tokens | Location |
|-------|-----------|--------|----------|
| Prompt Engineering | 7 | ~21K | `what/context/prompt_engineering/` |

Load via `what/context/AGENTS.md` → topic `AGENTS.md` → individual subtopics as needed.

> **Note**: The parent vault (LATTICE-PROTOCOL) maintains a 16-topic context library with ~195K+ tokens. The aDNA repo ships with 1 topic as a reference implementation.

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

| Component | Status | Description |
|-----------|--------|-------------|
| aDNA Standard v2.1 | Shipped | Core specification — triad, ontology (14 base + 8 extension), sessions, missions, campaigns |
| Context library | Shipped | 1 topic (prompt engineering), 7 subtopics, ~21K tokens |
| Lattice YAML tools | Shipped | Validate, convert (YAML↔canvas), JSON Schema, 6 example lattices |
| Mermaid-enhanced spec docs | Shipped | 19 diagrams across 3 aDNA specification documents |
| PRD/RFC pipeline | Shipped | 4-stage planning pipeline (research → requirements → design → review) |
| 10 templates | Shipped | Session, mission, campaign, context, ADR, backlog, coordination, PRD, RFC, skill |

See `how/missions/` for mission details and `STATE.md` for current operational state.
