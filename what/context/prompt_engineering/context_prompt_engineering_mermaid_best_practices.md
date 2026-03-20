---
type: context_research
topic: prompt_engineering
subtopic: mermaid_best_practices
created: 2026-02-19
updated: 2026-03-18
sources: ["Mermaid.js — Architecture Diagrams Docs", "hanyouqing.com — Mermaid Diagram Guide (2025)", "Obsidian Forum — Mermaid rendering discussions", "Anthropic — Context Engineering"]
context_version: "1.0"
token_estimate: ~1500
last_edited_by: agent_stanley
tags: [context, prompt_engineering]
quality_score: 3.6
signal_density: 4
actionability: 4
coverage_uniformity: 3
source_diversity: 3
cross_topic_coherence: 4
freshness_category: mixed
last_evaluated: 2026-03-17
---

# Prompt Engineering: Mermaid Diagram Best Practices

## Key Principles

1. **Diagrams complement text, not replace it.** Use diagrams to illustrate concepts difficult to explain in prose. Every diagram should have surrounding context explaining what it shows and why it matters.

2. **One concept per diagram.** Split complex systems into separate diagrams by concern. A system overview, a data flow, and an entity relationship model should be three diagrams, not one overloaded flowchart.

3. **Match diagram type to information type.** Each Mermaid diagram type excels at a specific communication pattern. Choosing wrong wastes tokens and confuses readers.

4. **Render targets constrain complexity.** GitHub, Obsidian, and Mermaid Live Editor have different rendering limits. Design for the most constrained target.

5. **Agents can author diagrams effectively.** Mermaid's text-based syntax is well-suited for agent generation — it's version-controllable, diffable, and requires no visual tools. But agents tend to over-complicate; constrain scope explicitly.

## Diagram Type Selection Guide

| Diagram Type | Best For | Avoid When |
|-------------|---------|------------|
| **Flowchart** | Process logic, decision trees, workflow steps | >15 nodes — split into subflows |
| **Sequence** | API flows, service interactions, temporal ordering | Static relationships (use class/ER instead) |
| **Class** | Entity relationships, type hierarchies, data models | Runtime behavior (use sequence/state) |
| **ER (Entity Relationship)** | Database schemas, ontology relationships, data models | Process flows (use flowchart) |
| **State** | Lifecycle states, status transitions, pipeline stages | Complex parallel flows (use flowchart with subgraphs) |
| **Architecture** | Cloud/deployment topology, service infrastructure (v11.1+) | Detailed data flow (use sequence) |
| **Gantt** | Timelines, project schedules, phase planning | Non-temporal information |
| **Git Graph** | Branch strategies, version history | Non-git workflows |
| **Pie Chart** | Proportional distribution, adoption metrics | Precise comparisons (use tables instead) |
| **User Journey** | End-to-end user flows, experience mapping | Technical architecture |

### Recommended Types for aDNA Documentation

| Document Type | Primary Diagram | Secondary |
|--------------|----------------|-----------|
| Ontology / entity relationships | ER diagram | Class diagram |
| Campaign/mission execution flow | Flowchart | State diagram |
| Agent cold-start chain | Sequence diagram | Flowchart |
| Pipeline stage progression | State diagram | Flowchart |
| System architecture | Architecture diagram | Flowchart with subgraphs |
| Context loading pattern | Sequence diagram | — |

## Recommendations

### Complexity Limits

| Target | Max Nodes | Max Edges | Notes |
|--------|----------|----------|-------|
| GitHub | ~30 nodes | ~40 edges | Renders inline in markdown; no scroll control |
| Obsidian | ~20-25 nodes | Default edge limit (configurable) | Diagrams can crop; horizontal scroll issues |
| Mermaid Live Editor | ~50+ nodes | Up to 2000 edges (configurable) | Most permissive; not a deployment target |

**Rule of thumb:** Keep diagrams under 20 nodes and 25 edges for reliable rendering across all targets. If a diagram exceeds this, split it.

### Formatting for Readability

- **Direction:** Use `TB` (top-to-bottom) for hierarchies, `LR` (left-to-right) for sequences and timelines
- **Labels:** Descriptive but short — 2-4 words per node label
- **Subgraphs:** Group related nodes to reduce visual complexity
- **Consistent naming:** Match node IDs to codebase terminology
- **Whitespace:** One blank line between logical sections of the diagram code

### Obsidian-Specific Considerations

- Diagrams may be cropped in reading view — test in both edit and reading modes
- CSS can adjust diagram container width: `.mermaid svg { max-width: 100%; }`
- The Mermaid Tools plugin (`obsidian-mermaid`) provides toolbar and improved editing
- Complex diagrams render better when the note has sufficient width (avoid narrow sidebars)
- Obsidian bundles a specific Mermaid version — newer syntax (e.g., architecture diagrams) may not be available until Obsidian updates

### GitHub-Specific Considerations

- Mermaid renders natively in `.md` files using ` ```mermaid ` code blocks
- No interactive features (zoom, pan) — diagram must be readable at default size
- GitHub may cache rendered diagrams — changes may not appear immediately in PR previews
- Theme follows GitHub's light/dark mode — avoid relying on specific colors

### Agent-Authored Diagram Patterns

When instructing agents to create diagrams:

```
Create a Mermaid [type] diagram showing [specific concept].
Keep it under 15 nodes. Use descriptive 2-3 word labels.
Direction: [TB/LR]. Include a brief text description above the diagram.
```

- Specify diagram type explicitly — agents default to flowcharts
- Set node count limits — agents tend to include every entity
- Request labels in domain terminology — prevents generic naming
- Ask for both the diagram and explanatory text

## Anti-Patterns

- **The kitchen-sink diagram.** 50+ nodes trying to show the entire system. Unreadable on any platform. Split by concern.
- **Flowcharts for everything.** Using flowcharts when sequence, state, or ER diagrams would communicate the concept more clearly.
- **Missing context.** A diagram with no surrounding text — reader doesn't know what to look for or why it matters.
- **Inconsistent terminology.** Node labels that don't match the codebase or documentation terms.
- **Over-styled diagrams.** Excessive colors, custom themes, and styling that don't render consistently across targets. Keep styling minimal.
- **Agents generating without constraints.** Without explicit limits, agents create diagrams that are technically correct but too complex to be useful.

## Sources

- [Architecture Diagrams Documentation](https://mermaid.js.org/syntax/architecture.html) — Mermaid.js (v11.1+). Architecture diagram syntax, groups, services, edges, junctions, icon support.
- [Complete Guide to Mermaid Diagrams in Technical Documentation](https://hanyouqing.com/blog/2025/08/mermaid-diagram-guide/) — Han Youqing (2025). 9 diagram types, selection guide, complexity management, documentation integration.
- [Mermaid JS Diagram Unreadable Due to Large Size](https://forum.obsidian.md/t/mermaid-js-diagram-unreadable-due-to-large-size/23769) — Obsidian Forum. Rendering limits, CSS workarounds, scaling constraints.
- [Obsidian maxEdges Discussion](https://forum.obsidian.md/t/is-there-a-way-to-increase-the-number-of-maxedges-for-mermaid-in-obsidian/81988) — Obsidian Forum. Edge count limits, configurable thresholds.
- [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic (2025). Agent-authored content patterns, signal density.
