---
type: directory_index
created: 2026-02-19
updated: 2026-02-19
last_edited_by: agent_stanley
tags: [directory_index, docs]
---

# what/docs/ — aDNA Specification Documents

## Purpose

Core specification documents that define the aDNA (Agentic DNA) knowledge architecture standard. These are the authoritative references for how aDNA vaults are structured, governed, and composed.

## Contents

| Document | Token Estimate | Purpose |
|----------|---------------|---------|
| `adna_standard.md` | ~8,000 | Normative aDNA specification v2.1 — triad structure, entity types, governance, pipelines, FAIR metadata. 9 Mermaid diagrams. |
| `adna_design.md` | ~5,000 | Design rationale and deployment patterns — standalone, nested, federated forms. 5 Mermaid diagrams. |
| `adna_bridge_patterns.md` | ~4,000 | Composition patterns for multi-vault architectures — nesting, federation, discovery. 5 Mermaid diagrams. |
| `context_quality_rubric.md` | ~2,000 | 6-axis quality evaluation framework for context files — scoring methodology, calibration examples. |

## Total Token Budget

~19,000 tokens to load all 4 documents. These are heavy reference documents — load specific files based on need.

## Load/Skip Decision

**Load this directory when**:
- Understanding or extending the aDNA standard itself (read `adna_standard.md`)
- Designing a new aDNA instance or evaluating deployment form (read `adna_design.md`)
- Planning multi-vault composition or federation (read `adna_bridge_patterns.md`)
- Evaluating context file quality or setting up review processes (read `context_quality_rubric.md`)

**Skip when**:
- Performing operational work within an established aDNA vault (sessions, missions, CRM)
- Working on domain-specific content that doesn't touch the aDNA standard
- Already familiar with the standard and not modifying its structure

**Token cost**: ~300 tokens (this AGENTS.md). Individual docs are 2,000-8,000 tokens each — load selectively.

## Cross-References

- [what/AGENTS](../AGENTS.md) — Knowledge layer index
- [what/context/AGENTS](../context/AGENTS.md) — Context library (builds on these specs)
- [CLAUDE.md](../../CLAUDE.md) — Governance (implements the standard)
