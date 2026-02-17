---
type: directory_index
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [directory_index, campaign]
---

# Campaigns

Multi-mission initiatives that coordinate large-scale work toward strategic goals.

## Purpose

Campaigns sit above missions in the work hierarchy. Where a mission decomposes a single large task into session-sized objectives, a campaign coordinates multiple missions toward a strategic goal. Campaigns provide:

- **Strategic framing** — a named initiative with clear goals, phases, and exit criteria
- **Mission sequencing** — dependency ordering across missions with phase gates
- **Planning integration** — R&D→PRD→RFC pipeline artifacts as campaign inputs
- **Progress tracking** — phase-level checkpoints and risk registers
- **Closeout rigor** — cross-system validation and follow-up campaign scoping

## Hierarchy

```
Campaign (strategic initiative, 10-40 sessions)
├── Phase (logical grouping of missions)
│   ├── Mission (multi-session task, 1-5 sessions each)
│   │   ├── Objective (session-sized unit of work)
│   │   └── Objective
│   └── Mission
│       └── Objective
└── Phase
    └── Mission
        └── Objective
```

## Directory Structure

```
how/campaigns/
├── AGENTS.md                           # This file (protocol)
└── campaign_<name>/                    # One directory per campaign
    ├── campaign_<name>.md              # Master campaign document
    └── ...                             # Supporting documents
```

Campaign missions live in `how/missions/` (not inside the campaign directory) with a `campaign_id` frontmatter field linking back to the campaign.

## Campaign Master Document Format

**Directory**: `how/campaigns/campaign_<name>/`
**Filename**: `campaign_<name>.md`

See `how/templates/template_campaign.md` for the full template.

Key frontmatter fields:

```yaml
---
campaign_id: campaign_<name>
type: campaign
title: "Human-readable campaign title"
owner: <username>
status: planning | active | completed | abandoned
phase_count: <N>
mission_count: <N>
estimated_sessions: "<range>"
created: YYYY-MM-DD
updated: YYYY-MM-DD
last_edited_by: agent_<username>
---
```

## Lifecycle

### 1. Planning

1. Identify a strategic initiative that requires multiple coordinated missions
2. Create R&D→PRD→RFC planning artifacts through `how/pipelines/prd_rfc/` (optional but recommended)
3. Create campaign directory at `how/campaigns/campaign_<name>/`
4. Write campaign master document using `template_campaign.md`
5. Set `status: planning`

### 2. Activation

1. Get user approval on campaign scope, phases, and mission sequence
2. Create mission files in `how/missions/` with `campaign_id` field
3. Set campaign `status: active`
4. Begin Mission 0 (infrastructure/setup) if defined

### 3. Execution

1. Execute missions in dependency order
2. Phase gates: verify exit criteria before advancing to next phase
3. Update campaign master document with progress after each mission
4. Checkpoint reviews at phase boundaries
5. Risk register updates as issues surface

### 4. Completion

1. Final validation mission (cross-system coherence)
2. Fill completion summary in campaign master document
3. Set `status: completed`
4. Scope follow-up campaigns if applicable
5. Update `how/STATE.md`

### 5. Abandonment

1. Set `status: abandoned` with rationale
2. Note which missions completed, which were dropped
3. File backlog ideas for any salvageable work

## Relationship to Other Systems

| System | Relationship |
|--------|-------------|
| **Missions** (`how/missions/`) | Campaigns contain missions. Missions reference their campaign via `campaign_id`. |
| **Sessions** (`how/sessions/`) | Sessions execute mission objectives. Sessions reference both mission and campaign. |
| **Pipelines** (`how/pipelines/`) | R&D→PRD→RFC pipeline produces planning artifacts that feed campaign design. |
| **Context Library** (`what/context/`) | Campaigns may create or consume context topics. |
| **Backlog** (`how/backlog/`) | Campaigns graduate from backlog ideas. Completed campaigns may generate new backlog ideas. |
| **STATE.md** (`how/STATE.md`) | Active campaigns tracked in operational state. |

## Rules

- **One campaign master document per initiative** — the master document is the single source of truth
- **Missions live in `how/missions/`** — not inside the campaign directory
- **Phase gates are human gates** — never auto-advance between campaign phases without user review
- **Campaign scope is mutable** — add, reorder, or remove missions as needed, but document scope changes
- **Subsumption** — when a campaign absorbs an existing mission, set the original mission's status to `subsumed`
- **Archive, don't delete** — campaign documents are permanent records even after completion
