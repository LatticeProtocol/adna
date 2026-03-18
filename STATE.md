---
type: state
created: 2026-02-17
updated: 2026-03-18
status: active
last_edited_by: agent_stanley
last_session: session_stanley_20260318_adna_review_phase2
tags: [state, governance]
---

# Operational State

Dynamic operational snapshot for cold-start orientation. Updated each session.

## Current Phase

**Production-validated.** aDNA v5.1 with hardened object standards, Canvas Standard v1.0.0, execution hierarchy v2 (OODA + AAR), and 14 example lattices across business, research, creative, personal, and biotech domains. aDNA Standard v2.2 (maintenance pass complete).

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
- Context library: 4 topics, 23 subtopics, ~58K tokens (prompt_engineering, adna_core, lattice_basics, object_standards)
- Sync protocol: vault↔repo `adna_core/` sync documented in vault skill (`how/skills/skill_adna_repo_sync.md`)
- Cross-topic recipe system: 6 domain-neutral recipes with 3-tier budgets
- Execution hierarchy v2: OODA cascade (3-level), AAR protocol (5-step), mission classes (5 types)
- Quality framework: 6-axis rubric, quality audit skill, context graduation pipeline
- Strategic compass template + escalation cascade (session→mission→campaign→STATE.md)
- 17 templates including AAR, strategic compass, campaign CLAUDE.md, registry, data record, folder note, PRD, RFC
- R&D→PRD→RFC planning pipeline (4 stages)
- Agent-driven onboarding (`how/skills/skill_onboarding.md`)
- 6 skills (onboarding, lattice publish, new entity type, context quality audit, context graduation, vault review)
- Session tracking, mission/campaign/backlog systems
- 10 CSS snippets for Obsidian visual polish

## Recent Decisions

| Date | Decision | Source |
|------|----------|--------|
| 2026-03-02 | Decision 9: YAML authoritative, canvas is view layer | campaign_adna_lattice M08 |
| 2026-03-02 | Decision 10: 19-type I/O vocabulary | campaign_adna_lattice M16 |
| 2026-03-02 | Decision 11: Nested FAIR canonical, flat FAIR transport | campaign_adna_lattice M16 |
| 2026-03-02 | Decision 12: Multi-cloud `.dataset.yaml` with FUSE | campaign_adna_lattice M16 |

## Recent Upgrades

| Date | Upgrade | Source |
|------|---------|--------|
| 2026-03-18 | CLAUDE.md v5.1 — lattice types table (7 values), template count (17), standard file path fixes | campaign_adna_comprehensive_review M08 |
| 2026-03-18 | aDNA Standard v2.2 — federation stub, vault extensions, campaign system, factual fixes | campaign_adna_comprehensive_review M07 |
| 2026-03-18 | Repo sync complete: 13 adna_core subtopics (was 10), 3 new files from vault sync | campaign_adna_comprehensive_review M01 |
| 2026-03-18 | README quickstart refined, FAQ section, plugin tiers documented | campaign_adna_comprehensive_review M03 |
| 2026-03-18 | Quality floor remediation: lattice_basics 3.0→4.0, object_standards 3.4→4.0 | campaign_adna_comprehensive_review M04 |
| 2026-03-17 | CLAUDE.md v5.0 — OODA cascade, AAR protocol, escalation cascade, context recipes | Framework port from lattice-labs |
| 2026-03-17 | 10 adna_core subtopics (was 8) — added ooda_cascade, ontology_workshop | Framework port |
| 2026-03-17 | Quality scoring on all 18 context files — 3 floor violations identified | Quality audit |
| 2026-03-17 | 4 new templates, 2 new skills, mission class discriminator | Framework port |

## Active Blockers

None.

## Next Steps

1. **Start an agent session** — run `claude` in the vault directory; Berthier guides interactive onboarding (~15-30 min)
2. **Or customize manually** — edit MANIFEST.md, STATE.md, and CLAUDE.md § Identity with your project identity (~30-45 min)
3. **Extend the ontology** — add domain-specific directories under who/what/how (see README § Extending the Ontology, or load `ontology_workshop` context)
4. **Explore the context library** — read `what/context/AGENTS.md` for topic index, or check `context_recipes.md` for pre-built assemblies
5. **Build a lattice** — copy an example from `what/lattices/examples/` and customize it
6. **Design a campaign** — use the `campaign_design` recipe to load campaign dispatch + convergence model context

## Partial-Resume Detection

If session history is non-empty but MANIFEST.md still shows `last_edited_by: agent_init`, onboarding was started but governance was not customized. Read `how/skills/skill_onboarding.md` and resume from the first step that hasn't produced output (check for customized MANIFEST.md, personalized CLAUDE.md § Identity, and updated STATE.md).
