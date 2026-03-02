---
type: state
created: 2026-02-17
updated: 2026-03-02
status: active
last_edited_by: agent_stanley
last_session: ""
tags: [state, governance]
---

# Operational State

Dynamic operational snapshot for cold-start orientation. Updated each session.

## Current Phase

**Production-validated.** aDNA v2.1 with hardened object standards, Canvas Standard v1.0.0, and 13 example lattices across business, research, creative, personal, and biotech domains.

## What's Working

- aDNA triad deployed (what/how/who, 5 governance files, 14 base entity types)
- Object standards hardened: module, dataset, lattice (targets as dataset subtype)
- Canvas Standard v1.0.0 with Round-Trip Protocol v1.0 (YAML authoritative, canvas as view layer)
- Type vocabulary: 19 canonical I/O types (Decision 10)
- FAIR metadata: flat↔nested envelope interconversion (Decision 11)
- `.dataset.yaml` schema: multi-cloud storage, 7 providers, FUSE support (Decision 12)
- Lattice YAML validation tool (`lattice_validate.py`) + JSON Schema
- Canvas-YAML bidirectional conversion (`lattice2canvas.py`, `canvas2lattice.py`)
- 13 example lattice files + 3 canvas templates + 1 demonstration canvas
- Prompt engineering context library (1 topic, 7 subtopics, ~21K tokens)
- R&D→PRD→RFC planning pipeline (4 stages)
- Agent-driven onboarding (`how/skills/skill_onboarding.md`)
- 10 templates, session tracking, mission/campaign/backlog systems
- 10 CSS snippets for Obsidian visual polish

## Recent Decisions

| Date | Decision | Source |
|------|----------|--------|
| 2026-03-02 | Decision 9: YAML authoritative, canvas is view layer | campaign_adna_lattice M08 |
| 2026-03-02 | Decision 10: 19-type I/O vocabulary | campaign_adna_lattice M16 |
| 2026-03-02 | Decision 11: Nested FAIR canonical, flat FAIR transport | campaign_adna_lattice M16 |
| 2026-03-02 | Decision 12: Multi-cloud `.dataset.yaml` with FUSE | campaign_adna_lattice M16 |

## Active Blockers

None.

## Next Steps

1. **Start an agent session** — run `claude` in the vault directory; Berthier guides interactive onboarding (~15-30 min)
2. **Or customize manually** — edit MANIFEST.md, STATE.md, and CLAUDE.md § Identity with your project identity (~30-45 min)
3. **Extend the ontology** — add domain-specific directories under who/what/how (see README § Extending the Ontology)
4. **Explore the context library** — read `what/context/prompt_engineering/AGENTS.md` for context engineering best practices
5. **Build a lattice** — copy an example from `what/lattices/examples/` and customize it

## Partial-Resume Detection

If session history is non-empty but MANIFEST.md still shows `last_edited_by: agent_init`, onboarding was started but governance was not customized. Read `how/skills/skill_onboarding.md` and resume from the first step that hasn't produced output (check for customized MANIFEST.md, personalized CLAUDE.md § Identity, and updated STATE.md).
