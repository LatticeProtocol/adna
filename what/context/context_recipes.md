---
type: context_guide
topic: context_library
subtopic: recipes
created: 2026-03-17
updated: 2026-03-17
sources: ["Context library AGENTS.md", "Context engineering guide", "Campaign dispatch guide"]
context_version: "1.0"
token_estimate: ~2000
last_edited_by: agent_stanley
tags: [context, recipes, assembly, cross-topic]
---

# Context Library: Cross-Topic Assembly Recipes

Named, pre-defined cross-topic context assemblies for multi-disciplinary tasks. Each recipe specifies which subtopics to load, in what order, with token budgets at three tiers.

## Key Principles

1. **Recipes are shortcuts, not requirements** — An agent can always load subtopics manually. Recipes encode known-good combinations.
2. **Three budget tiers** — Minimal (quick tasks), Standard (most work), Full (comprehensive production). Follow convergent narrowing: start narrow, expand only if needed.
3. **Order matters** — Subtopics listed in recommended load order. Foundational files first, detail files second.

## Recipe Index

### Core Domain

| Recipe | Topics Used | Subtopics (Standard Load) | ~Tokens | Use When |
|--------|------------|---------------------------|---------|----------|
| `ontology_extension` | adna_core + prompt_engineering | paradigm_overview, ontology_design, PE/ontology_design | ~9K | Extending the base ontology with domain-specific entities |
| `campaign_design` | adna_core | campaign_dispatch, convergence_model | ~5K | Designing a new campaign — phases, missions, scope |
| `context_authoring` | adna_core | context_engineering, signal_to_token | ~5K | Writing new context files following quality standards |
| `lattice_authoring` | adna_core + lattice_basics | lattice_design, core_concepts | ~8K | Creating lattice YAML files, node/edge design |
| `cold_start_orientation` | adna_core | paradigm_overview, convergence_model | ~5.5K | Bootstrapping into aDNA — first session orientation |
| `federation_design` | adna_core | federation, lattice_design, ontology_unification | ~10K | Designing cross-instance federation, URI schemes, merge protocols |

### Extended Domain

Add domain-specific recipes here as your context library grows. Follow the Recipe Creation Protocol below.

## Token Budget Tiers

Each recipe supports three budget tiers. The Standard load is shown in the index above.

| Tier | When to Use | Budget Rule |
|------|-------------|-------------|
| **Minimal** | Quick reference, single-task checks | Load only the first 1-2 subtopics from the recipe. Target <5K tokens |
| **Standard** | Most tasks — session work, mission objectives | Load the subtopics shown in the recipe index. Target <12K tokens |
| **Full** | Comprehensive production, campaign-level work | Load all subtopics from all listed topics. Budget the full topic totals |

### Tier Examples (ontology_extension)

| Tier | Load | ~Tokens |
|------|------|---------|
| Minimal | paradigm_overview | ~3K |
| Standard | paradigm_overview, ontology_design, PE/ontology_design | ~9K |
| Full | All 8 adna_core + all 7 prompt_engineering | ~45K |

## Task Classification Heuristic

When no recipe is explicitly requested, match the task to a recipe using these keywords:

| Keywords in Task | Recipe Match |
|-----------------|--------------|
| ontology, entity, extend, domain type, namespace | `ontology_extension` |
| campaign, phase, mission, strategic initiative | `campaign_design` |
| context file, context topic, write context, quality rubric | `context_authoring` |
| lattice, YAML, node, edge, workflow, pipeline | `lattice_authoring` |
| cold start, bootstrap, first run, orientation | `cold_start_orientation` |
| federation, merge, cross-instance, URI, seam | `federation_design` |

If no match: fall back to manual subtopic selection from the relevant topic AGENTS.md files.

## Recipe Creation Protocol

When adding a new recipe:

1. **Identify the recurring pattern** — the recipe must serve 2+ tasks or be used by a skill/pipeline
2. **List subtopics in load order** — foundational first, detail second
3. **Calculate token budgets** — sum `token_estimate` fields from each subtopic's frontmatter
4. **Define all three tiers** — Minimal, Standard, Full
5. **Add keyword triggers** to the Task Classification Heuristic table
6. **Update this file** — add the recipe row to the appropriate domain section

## Sources

- Context library AGENTS.md — topic index and token estimates
- Context engineering guide (`context_adna_core_context_engineering.md`) — quality standards and budget principles
- Campaign dispatch guide (`context_adna_core_campaign_dispatch.md`) — execution hierarchy patterns
