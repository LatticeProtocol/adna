---
type: decision
adr_id: adr_003
adr_number: 3
title: "System Configuration as Cross-Triad Domain Topic"
status: proposed
created: 2026-03-27
updated: 2026-03-27
last_edited_by: agent_aria
supersedes:
superseded_by:
tags: [adr, decision, system_configuration, context, triad]
---

# ADR-003: System Configuration as Cross-Triad Domain Topic

## Status

Proposed

## Context

The aDNA context library documents the knowledge architecture itself (adna_core, prompt_engineering, lattice_basics, object_standards) but had no context about the operating environment agents run in — the Claude Code infrastructure that enables the knowledge architecture to function.

This infrastructure includes: configuration inheritance (CLAUDE.md cascade), lifecycle hooks, persistent agent memory (AgentDB), multi-vault organization, auto-memory integration, orchestration tiers, and agent behavioral protocol.

The challenge: this domain spans all three triad legs. Agent behavioral rules are governance (WHO). Orchestration procedures are operations (HOW). Technical knowledge about hooks, AgentDB, and memory layers is knowledge (WHAT). Placing everything in one leg violates the question test.

Alternatives considered:
1. **Everything in what/context/** — simple but violates triad placement (governance and operations mixed into knowledge)
2. **Distribute across triad with no unifying structure** — correct placement but no way to navigate the topic as a whole
3. **Distribute across triad + context_graph lattice** — correct placement AND a navigable composition graph

## Decision

Adopt option 3: distribute system configuration content across the triad according to the question test, and create a `context_graph` lattice that models the relationships between components.

Placement:
- **who/governance/governance_agent_protocol.md** — agent behavioral contract (WHO decides how agents behave?)
- **how/skills/skill_orchestration_tiers.md** — tier-based execution routing (HOW does work get done?)
- **what/context/system_configuration/** — 5 context files for technical knowledge (WHAT is the config system? the hook system? AgentDB? memory? vault architecture?)
- **what/lattices/examples/system_configuration.lattice.yaml** — context_graph lattice connecting all 8 components with directed edges showing dependency flow

The lattice provides the unified navigation layer: each node references its triad-correct file via `ref`, and edges show how config flows into hooks, hooks trigger AgentDB, AgentDB feeds memory, etc.

## Consequences

### Positive
- Respects the question test — every file lives in the triad leg that matches its primary question
- The context_graph lattice provides navigability despite distribution — agents can load the lattice to understand the full system topology
- Demonstrates a reusable pattern: cross-triad domains can be modeled as lattices with nodes pointing to files across the triad
- Complements existing adna_core context — the `adna_triad` node in the lattice explicitly connects to the paradigm overview

### Negative
- Higher complexity than a single context topic — 3 different file types (governance doc, skill, context files) instead of one
- The lattice is a context_graph (knowledge structure), not executable — may set expectations of runtime behavior
- Agents must load the lattice to understand the full picture; individual files are self-contained but don't show relationships

### Neutral
- Establishes a precedent for how future cross-triad domains should be structured
- The context_graph lattice type gets a concrete example beyond the existing knowledge_base.lattice.yaml
