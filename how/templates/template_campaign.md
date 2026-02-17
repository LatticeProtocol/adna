---
campaign_id: campaign_<name>
type: campaign
title: "Human-readable campaign title"
owner: <username>
status: planning
phase_count: <N>
mission_count: <N>
estimated_sessions: "<min>-<max>"
priority: high | medium | low
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
last_edited_by: agent_<username>
tags: [campaign]
---

# Campaign: <% tp.file.title %>

## Goal

[One-paragraph strategic objective. What does the world look like when this campaign is complete?]

## Context

[Why this campaign exists. What triggered it. What prior work it builds on. Link to R&D artifacts, backlog ideas, or strategic decisions.]

## Scope

### In Scope

- [Major deliverable or capability 1]
- [Major deliverable or capability 2]

### Out of Scope

- [Explicitly excluded work and rationale]

### Subsumes

| Plan/Mission | Status at Subsumption | Tasks Absorbed By |
|-------------|----------------------|-------------------|
| [[plan_or_mission]] | active (tasks N-M remaining) | Missions X, Y |

## Phases & Missions

### Phase 1: [Phase Name]

| Mission | Title | Sessions | Dependencies | Status |
|---------|-------|----------|-------------|--------|
| 0 | [Mission title] | 2-3 | — | planned |
| 1 | [Mission title] | 2 | M0 | planned |

**Phase exit gate**: [What must be true before Phase 2 begins]

### Phase 2: [Phase Name]

| Mission | Title | Sessions | Dependencies | Status |
|---------|-------|----------|-------------|--------|
| 2 | [Mission title] | 2 | M1 | planned |

**Phase exit gate**: [Exit criteria]

## Decision Points

| # | When | Decision | Status |
|---|------|----------|--------|
| 1 | Before Mission N | [What needs user approval] | pending |

## Risk Register

| Risk | Severity | Mitigation |
|------|----------|------------|
| [Risk description] | Critical / High / Medium / Low | [Mitigation strategy] |

## Verification Strategy

### Per-Mission

- SITREP with completed/in-progress/next-up/blockers/files-touched

### Per-Phase

- [Phase 1 checkpoint criteria]
- [Phase 2 checkpoint criteria]

### Campaign Validation

- [Final validation approach -- cross-system coherence, audits, etc.]

## Timeline

| Phase | Missions | Sessions |
|-------|----------|----------|
| Phase 1 | 0-N | X-Y |
| **Total** | **N missions** | **X-Y sessions** |

## Notes

[Cross-cutting observations, architectural decisions, coordination notes]

## Completion Summary

*Fill out when setting `status: completed`.*

### Deliverables
- [Concrete outputs: repos, files, systems, records]

### Descoped
- [Missions skipped or deferred, with justification]

### Key Findings
- [Insights and discoveries from campaign execution]

### Scope Changes
- [Missions added or removed during execution, and why]

### Follow-Up Campaigns
- [Scoped follow-up initiatives]
