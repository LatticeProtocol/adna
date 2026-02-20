---
type: state
created: 2026-02-17
updated: 2026-02-19
status: active
last_edited_by: agent_stanley
last_session: session_stanley_20260219_adna_m6
tags: [state, governance]
---

# Operational State

Dynamic operational snapshot for cold-start orientation. Updated each session.

## Current Phase

**Ready — Complete Template.** The lattice-adna repo ships as a fully functional aDNA quick-start with validated tools, context library, Mermaid-enhanced spec docs, and agent-driven onboarding. Clone and start building.

## Recent Decisions

| Date | Decision | Source |
|------|----------|--------|
| 2026-02-19 | AGENTS.md enrichment — 5 new files, 20 enriched with load/skip guidance, 25 total | campaign_adna_review M6 |
| 2026-02-19 | CLAUDE.md v4.0 — signal-to-token optimization, convergence model, ontology table | campaign_adna_review M5 |
| 2026-02-19 | 19 Mermaid diagrams added to 3 spec docs | campaign_adna_review M4 |
| 2026-02-19 | 6-axis context quality rubric deployed | campaign_adna_review M2 |
| 2026-02-19 | Prompt engineering context library (7 subtopics) created | campaign_adna_review M1 |
| 2026-02-17 | Initial repo created with aDNA bare triad + lattice tools | campaign_lattice_integration M8 |

## Active Blockers

None.

## What's Working

- 25 AGENTS.md files with convergence-aware load/skip guidance across all content directories
- aDNA skeleton deployed (what/how/who triad, governance files, 10 templates)
- Prompt engineering context library (1 topic, 7 subtopics, ~21K tokens)
- Mermaid-enhanced spec docs (19 diagrams across 3 documents)
- Context quality evaluation framework (6-axis rubric with scoring template)
- Lattice YAML validation tool (`lattice_validate.py`)
- Canvas-YAML bidirectional conversion (`lattice2canvas.py`, `canvas2lattice.py`)
- JSON Schema for `.lattice.yaml` format
- 3 example lattice files (deep research, orchestrator, protein binder)
- R&D→PRD→RFC planning pipeline with 4 stages
- Session tracking ready (`how/sessions/active/`)
- Mission, campaign, backlog, and skills systems ready
- 10 CSS snippets for Obsidian visual polish
- Agent-driven onboarding (`how/skills/skill_onboarding.md`)

## Next Steps

1. **Start an agent session** — run `claude` in the vault directory; Berthier guides interactive onboarding (~5 min)
2. **Or customize manually** — edit MANIFEST.md, STATE.md, and CLAUDE.md § Identity with your project identity
3. **Create your first lattice** — copy an example from `what/lattices/examples/` and customize
4. **Explore the context library** — read `what/context/prompt_engineering/AGENTS.md` for context engineering best practices
5. **Start a mission** — use `how/missions/` for multi-session work decomposition

## Partial-Resume Detection

If session history is non-empty but MANIFEST.md still shows `last_edited_by: agent_init`, onboarding was started but governance was not customized. Read `how/skills/skill_onboarding.md` and resume from the first step that hasn't produced output (check for customized MANIFEST.md, personalized CLAUDE.md § Identity, and updated STATE.md).
