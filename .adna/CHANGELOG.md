# Changelog

All notable changes to the aDNA knowledge architecture are documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## Version Policy

aDNA tracks **two independent version numbers**:

| Track | File | Field | What it covers |
|-------|------|-------|---------------|
| **Governance** | `CLAUDE.md` | `version` (frontmatter) | Vault structure, agent protocol, safety rules, templates, skills |
| **Standard** | `what/docs/adna_standard.md` | Document title | Normative specification — triad structure, object schemas, FAIR metadata |

Both tracks use **Major.Minor** versioning (no patch level):

| Change type | Major bump | Minor bump |
|-------------|-----------|------------|
| **Governance** | Breaking changes to vault structure, CLAUDE.md format, or frontmatter schema | New features, templates, skills, context topics, non-breaking additions, corrections |
| **Standard** | Breaking changes to triad structure, object schemas, or FAIR envelope format | New sections, clarifications, federation stubs, factual fixes |

**Canonical version location**: `CLAUDE.md` frontmatter `version` field (governance track). The migration system (`how/migrations/`) uses this field for pre-flight checks.

Changelog entries are organized by **governance version** (primary heading). Standard version changes are noted within entries when they coincide.

---

## [v6.0] — 2026-04-03

### Changed
- **Identity rename**: Display name `adna` → `Agentic-DNA` across all governance files, docs, and context. GitHub repo renamed to `LatticeProtocol/Agentic-DNA`.
- **Naming convention**: `Agentic-DNA` = display name, headings, repo name; `aDNA` = abbreviation in running text; `adna` lowercase preserved in protocol URIs, directory names, tags, and frontmatter topics.
- `.aDNA` directory convention: `adna_standard.md` §3.5 with RFC 2119 naming rules, `skill_project_fork.md` auto-appends `.aDNA` suffix, CLAUDE.md workspace detection uses `*.aDNA/` glob.
- Clone command updated: `git clone https://github.com/LatticeProtocol/Agentic-DNA.git adna` (local dir stays `adna/`).
- Version bump: CLAUDE.md `5.7` → `6.0`.
- `how/missions/AGENTS.md`: Mandatory AAR before `status: completed`, git-aware state verification.
- `how/campaigns/AGENTS.md`: Verifiable DG criteria, campaign AAR step, context graduation sequencing.
- CLAUDE.md: 6 standing orders + git coordination section (5 rules + truth hierarchy).
- `template_mission.md`: AAR section appended.
- `template_campaign.md`: `calibrated_sessions`, `estimation_class` fields added, campaign AAR section.
- `lattice_yaml_schema.json`: `lattice_type` enum expanded 4→7 (added `skill`, `infrastructure`, `context_set`).
- `adna_standard.md` §7.2: Required base fields updated 5→6 (added `status`).
- Template count: 20→22.

### Fixed
- `CONTRIBUTING.md`: `validate_lattice.py` → `lattice_validate.py` (correct script name).
- `CONTRIBUTING.md`: Removed reference to nonexistent `calibrate_token_estimates.sh`.
- `skill_sqlite_persistence.md`: Added `status: proposed` to frontmatter (was missing status semantics).
- `governance_agent_protocol.md`: Added `runtime: claude_code` to frontmatter for clarity.
- Count reconciliation: context library 4→5 topics, 23→27 subtopics; adna_core 10→13; skills 7→13; example lattices 14→15; standard version v2.1→v2.2 references corrected.
- `adna_validate.py`: Frontmatter MUST violations now reported as errors (were warnings).
- `canvas2lattice.py`: `_sanitize_node_id()` converts canvas UUIDs to valid lattice IDs.
- Schema/validator/spec triple divergence resolved — all three sources now agree on 7 lattice types and 6 required fields.

### Added
- `template_aar_lightweight.md` — 5-line AAR format (Worked/Didn't/Finding/Change/Follow-up).
- `template_campaign_mission.md` — campaign-linked mission template with campaign_id, exit gate, AAR.
- PR #1 integration acknowledgment (ariaxhan system_configuration contribution).

---

## [v5.7] — 2026-03-23

### Added
- `role: template` marker in `MANIFEST.md` frontmatter — distinguishes the base template from forked projects
- `how/skills/skill_project_fork.md` — dedicated skill for forking `adna/` into a new project directory
- `~/lattice/` as the canonical workspace convention for L0 nodes
- L0 compute tier in CLAUDE.md Compute Tiers table (local knowledge architecture, no compute services)
- Workspace convention diagram in CLAUDE.md Template Detection section

### Changed
- `CLAUDE.md` v5.6→v5.7: merged "First-Run Detection" + "Workspace Bootstrap Detection" into unified "Template Detection & Project Setup" flow
- `skill_onboarding.md` now runs exclusively in forked projects, never in the base template
- `skill_workspace_init.md` Step 4 delegates project creation to `skill_project_fork.md`
- All documentation updated from `~/Projects/` to `~/lattice/` as the recommended workspace root
- `README.md` Quick Start updated with `~/lattice/` clone instructions and template-aware setup flow
- `projects_folder_pattern.md` updated with `~/lattice/` as canonical workspace root and `role: template` design principle
- `workspace_claude_md.template` updated with fork preparation steps (strip `role: template`, set `agent_init`)
- `STATE.md` next steps updated with `~/lattice/` convention
- Peripheral files updated: `skill_l1_upgrade.md`, `skill_lattice_publish.md`, `tutorial_lattice_publishing.md`, `tools/AGENTS.md`, `quest_l1_onboarding.md`

### Design decisions
- `adna/` stays clean — never customized by onboarding. `git pull` always safe.
- `role: template` in MANIFEST.md is the canonical detection mechanism (explicit, git-independent)
- `~/lattice/` is a strong recommendation, not mandatory — system works in any location

---

## [v5.5] — 2026-03-20

### Added
- `VISION.md` — ecosystem vision document (decentralized frontier lab model, participation ladder)
- "Ecosystem & Vision" section in `README.md` linking to VISION.md
- `VISION.md` row in README Further Reading table

### Changed
- `CLAUDE.md` version bump: `5.4` → `5.5`
- Template counts corrected across all governance files: `17` → `20` (CLAUDE.md, MANIFEST.md, README.md, STATE.md)
- `MANIFEST.md` template table expanded with 6 missing rows (data_record, folder_note, governance, migration, side_quest, quest_result)
- `MANIFEST.md` architecture tree updated with `community/` directory
- `README.md` tree diagram updated with `community/` directory
- `STATE.md` updated to v5.5, added community infrastructure to What's Working, added v5.3-v5.5 upgrade entries

---

## [v5.4] — 2026-03-20

### Added
- `community/quests/` — side-quest experiment specifications directory
- `community/results/` — structured result submissions directory
- `community/tools/aggregate_results.py` — reference aggregation script (stdlib only)
- `community/AGENTS.md` — community directory agent guide
- `how/templates/template_side_quest.md` — quest specification template
- `how/templates/template_quest_result.md` — result submission template
- `what/docs/side_quest_guide.md` — participation guide (find, run, submit quests)
- 2 example quests: `quest_frontmatter_comparison` (medium), `quest_migration_smoke_test` (easy)
- Side-Quest Awareness section in `CLAUDE.md`

### Changed
- `CLAUDE.md` version bump: `5.3` → `5.4`
- `AGENTS.md` project structure updated with `community/` directory
- `README.md` contributing section updated with side-quest mention

---

## [v5.3] — 2026-03-20

### Added
- `how/skills/skill_upstream_contribution.md` — agentic upstream contribution protocol
- Upstream Contribution Awareness section in `CLAUDE.md`
- Upstream Contribution Awareness section in root `AGENTS.md`
- Contributing section in `README.md`

### Changed
- `CONTRIBUTING.md` revised — Agent Contribution Mode replaces static PR-Context pattern; organic discovery over structured proposals
- `CLAUDE.md` version bump: `5.2` → `5.3`
- Code of Conduct reference updated to forward-looking note (pending `CODE_OF_CONDUCT.md`)

### Removed
- Empty `community/proposals/` directory (superseded by agentic backlog pattern)
- Empty `.github/ISSUE_TEMPLATE/` directory (standard GitHub issues sufficient for now)

### Tooling (non-versioned, built in lattice-labs campaign LSU)
- `compliance_checker.py` — 10-dimension compliance scoring for vault objects (~1085 LOC)
- 4 object migration prompts: `migrate_object_{skill,module,dataset,lattice}.md`
- `migration_safety_framework.md` — git tags, worktree testing, rollback L1-L5
- `adna_validate.py` — instance conformance validator per §5.5
- `frontmatter_schema.json` — JSON Schema for frontmatter validation
- Post-LSU fixes: D9 companion content validation, MCP module_type D5 N/A

---

## [v5.2] — 2026-03-19

### Added
- `CHANGELOG.md` — centralized version history (this file)
- Version policy documenting the two-track scheme (governance + standard)
- Migration prompt cross-links in changelog entries

### Changed
- `CLAUDE.md` version bump: `5.1` → `5.2`
- `STATE.md` updated with v5.2 reference and CHANGELOG in "What's Working"

### Migration
- [`migrate_v5.1_to_v5.2.md`](how/migrations/migrate_v5.1_to_v5.2.md)

---

## [v5.1] — 2026-03-18

### Changed
- Lattice types table expanded (7 values: added `infrastructure`, `context_set`, `skill`)
- Template count corrected (10 → 17)
- Standard file paths fixed (pointed to context library instead of nonexistent standalone files)

### Fixed
- CLAUDE.md token estimate corrected (`~650` → `~2500`)
- Object standards table references corrected

### Migration
- [`migrate_v5.0_to_v5.1.md`](how/migrations/migrate_v5.0_to_v5.1.md)

### Standard
- **v2.2** — Federation stub, vault extensions, campaign system, factual fixes

---

## [v5.0] — 2026-03-17

### Added
- OODA cascade (3-level: session, mission, campaign)
- AAR protocol (5-step)
- Escalation cascade (session → mission → campaign → STATE.md)
- Context recipes (6 domain-neutral, 3-tier budgets)
- Mission class discriminator (5 types: build, investigate, design, review, operate)
- 4 new templates (AAR, strategic compass, campaign CLAUDE.md, registry)
- 2 new skills (context quality audit, context graduation)

### Changed
- Framework port from lattice-labs vault to standalone aDNA repo
- 10 adna_core subtopics (was 8) — added ooda_cascade, ontology_workshop

---

## [v4.x and earlier] — 2026-02 to 2026-03

Pre-versioned history. Key milestones (not individually versioned):

- **Lattice publishing** — `latlab lattice publish/pull/compose` CLI workflow
- **Context library** — 4 topics, 23 subtopics, ~58K tokens
- **Object standards** — module, dataset, lattice type standards hardened
- **Canvas Standard v1.0.0** — Round-Trip Protocol v1.0 (YAML authoritative)
- **Type vocabulary** — 19 canonical I/O types (Decision 10)
- **FAIR metadata** — flat↔nested envelope interconversion (Decision 11)
- **Dataset schema** — multi-cloud `.dataset.yaml` with 7 providers (Decision 12)
- **Onboarding skill** — agent-driven interactive vault setup
- **14 example lattices** — business, research, creative, biotech domains
- **Obsidian config** — Tokyo Night theme, 10 CSS snippets, 14 plugins
