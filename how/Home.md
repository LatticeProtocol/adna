---
type: dashboard
name: "Project Home"
tags: [dashboard, homepage]
cssclasses: [dashboard]
status: active
created: 2026-02-19
updated: 2026-02-19
last_edited_by: agent_stanley
---

# lattice-adna

*Design, validate, and iterate on Lattice YAML definitions with AI agents.*

> An AI-native knowledge vault built on the **aDNA paradigm** — where humans browse in Obsidian and agents operate via Claude Code.

---

## Operational State

> [!info] Current Phase
> **Phase 0 — Setup.** Vault initialized from the aDNA-Obsidian starter kit. Customize `CLAUDE.md`, `MANIFEST.md`, and `STATE.md` with your project identity, then begin your first campaign.
>
> [[STATE|Operational State]] | [[MANIFEST|Project Overview]] | [[CLAUDE|Agent Context]]

---

## Recent Sessions

```dataview
TABLE WITHOUT ID
  link(file.path, session_id) AS "Session",
  intent AS "Focus",
  status AS "Status"
FROM "how/sessions"
WHERE type = "session"
SORT created DESC
LIMIT 5
```

---

## Active Campaigns

```dataview
TABLE WITHOUT ID
  link(file.path, title) AS "Campaign",
  status AS "Status",
  owner AS "Owner"
FROM "how/campaigns"
WHERE type = "campaign" AND status != "completed"
SORT created DESC
```

---

## Vault Overview

| Layer | Files | Purpose |
|-------|-------|---------|
| **what/** | `$= dv.pages('"what"').length` | Knowledge objects, context library, lattice tools |
| **how/** | `$= dv.pages('"how"').length` | Missions, sessions, campaigns, templates, pipelines |
| **who/** | `$= dv.pages('"who"').length` | People, teams, coordination, governance |

---

## Quick Start

> [!abstract] Getting Started
> 1. **Customize governance** — Edit `CLAUDE.md`, `MANIFEST.md`, and `STATE.md` with your project details
> 2. **Create your first campaign** — Use the campaign template in `how/campaigns/`
> 3. **Start a session** — Create a session file in `how/sessions/active/`
> 4. **Build context** — Add knowledge topics to `what/context/`

### Templates

| Template | Location | Creates in |
|----------|----------|-----------|
| Session | `how/templates/template_session.md` | `how/sessions/active/` |
| Mission | `how/templates/template_mission.md` | `how/missions/` |
| Campaign | `how/templates/template_campaign.md` | `how/campaigns/` |
| Context Entry | `how/templates/template_context.md` | `what/context/` |

---

## Navigation

> [!quote]
> **[[what/AGENTS|Knowledge (what/)]]** — what you know, what you've learned, what you've decided
> **[[how/AGENTS|Operations (how/)]]** — how you work, missions, sessions, templates
> **[[who/AGENTS|Organization (who/)]]** — who's involved, coordination, governance

---

## Key Documents

| Document | Purpose |
|----------|---------|
| [[CLAUDE]] | Agent master context — project identity, safety rules, protocol |
| [[MANIFEST]] | Project overview — architecture, entry points, active builds |
| [[STATE]] | Operational state — current phase, blockers, next steps |
| [[AGENTS]] | Root agent guide — directory overview, naming, startup |

---

<div class="homepage-footer">

lattice-adna — powered by the [aDNA paradigm](https://github.com/lat-labs/adna-obsidian)

</div>
