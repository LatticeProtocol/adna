---
type: directory_index
created: 2026-03-27
updated: 2026-03-27
token_estimate: 12500
last_edited_by: agent_aria
tags: [directory_index, context, system_configuration]
---

# System Configuration — Context Index

## Overview

Technical knowledge about the Claude Code operating environment for AI-native projects — vault architecture, configuration inheritance, lifecycle hooks, operational memory, and the four-layer persistence model. Generic patterns applicable to any aDNA deployment.

This topic is part of a **cross-triad domain** (see ADR-003). Governance and operational components live in their triad-correct locations; this directory holds the WHAT (technical knowledge) portion:

| Component | Triad | Location |
|-----------|-------|----------|
| Agent protocol | WHO | `who/governance/governance_agent_protocol.md` |
| Orchestration tiers | HOW | `how/skills/skill_orchestration_tiers.md` |
| Vault architecture | WHAT | this directory |
| Config cascade | WHAT | this directory |
| Hook system | WHAT | this directory |
| AgentDB | WHAT | this directory |
| Memory integration | WHAT | this directory |
| **Full topology** | — | `what/lattices/examples/system_configuration.lattice.yaml` |

## Subtopics

| # | Subtopic | File | ~Tokens | Subtype | Key Content |
|---|----------|------|---------|---------|-------------|
| 1 | Vault Architecture | `context_system_configuration_vault_architecture.md` | ~2,500 | context_guide | Multi-vault separation, _meta/ routing, .claude/ read-only, workspace convention |
| 2 | Config Cascade | `context_system_configuration_config_cascade.md` | ~2,000 | context_core | CLAUDE.md inheritance, rules modularization, settings layering, token budgets |
| 3 | Hook System | `context_system_configuration_hook_system.md` | ~2,500 | context_guide | 6 lifecycle events, guard pattern, auto-approve, pre-compaction, async vs blocking |
| 4 | AgentDB | `context_system_configuration_agentdb.md` | ~2,500 | context_guide | SQLite memory, absolute paths, multi-level isolation, learnings, contracts, schema |
| 5 | Memory Integration | `context_system_configuration_memory_integration.md` | ~3,000 | context_core | Four persistence layers, auto-memory vs CLAUDE.md, learnings vs AgentDB boundaries |

## Total Token Budget

~12,500 tokens to load all subtopics. Typical session loads 2-3 subtopics (~4K-5K tokens).

For the full system picture (~16K tokens), also load:
- `who/governance/governance_agent_protocol.md` (~1,900 tokens)
- `how/skills/skill_orchestration_tiers.md` (~2,100 tokens)

## Usage by Task

| Task | Load |
|------|------|
| Setting up a new workspace | vault_architecture, config_cascade |
| Configuring agent behavior | `who/governance/governance_agent_protocol.md`, config_cascade |
| Adding lifecycle hooks | hook_system, config_cascade |
| Setting up AgentDB | agentdb, memory_integration |
| Planning multi-agent work | `how/skills/skill_orchestration_tiers.md`, agentdb |
| Understanding where to store what | memory_integration, vault_architecture |
| Debugging config inheritance | config_cascade, hook_system |
| Onboarding a new agent | `who/governance/governance_agent_protocol.md`, vault_architecture, memory_integration |
| Auditing memory health | agentdb, memory_integration |
| Navigating full topology | `what/lattices/examples/system_configuration.lattice.yaml` |

## Dependency Notes

- **vault_architecture** and **config_cascade** are complementary — architecture defines structure, cascade defines how configuration flows through that structure
- **hook_system** depends on config_cascade — hooks are configured in settings files
- **agentdb** is self-contained — SQLite schema and patterns
- **memory_integration** is the capstone — references all other components to explain how the layers compose
- The **system_configuration.lattice.yaml** context graph models all 8 components (including cross-triad ones) as nodes with directed edges showing dependency flow

## Load/Skip Decision

**Load when**:
- Setting up or configuring a new aDNA workspace or vault
- Troubleshooting hook failures, config inheritance, or memory issues
- Onboarding a new agent or explaining the operating environment

**Skip when**:
- Working on domain-specific content (ontologies, lattices, campaigns)
- Performing routine session work in an already-configured vault
- Need agent protocol only → load `who/governance/` directly
- Need orchestration only → load `how/skills/skill_orchestration_tiers.md` directly

**Token cost**: ~500 tokens (this AGENTS.md)
