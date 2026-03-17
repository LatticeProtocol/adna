---
type: context_core
topic: lattice_basics
subtopic: core_concepts
created: 2026-03-05
updated: 2026-03-05
sources: ["adna CLAUDE.md", "aDNA Standard v2.1"]
context_version: "1.0"
token_estimate: ~2500
last_edited_by: agent_stanley
tags: [context, lattice_basics]
quality_score: 3.0
signal_density: 3
actionability: 2
coverage_uniformity: 3
source_diversity: 1
cross_topic_coherence: 4
freshness_category: stable
last_evaluated: 2026-03-17
---

# Lattice Basics: Core Concepts

## What is aDNA?

**aDNA** (Autonomous DNA) is a framework for organizing human-AI collaborative work. It provides:

1. **A vault structure** (who/what/how triad) — the knowledge graph
2. **Object types** (modules, datasets, lattices) — composable building blocks
3. **Operational protocols** (campaigns, missions, sessions) — structured execution
4. **Agent integration** — AI agents operate alongside humans in the same knowledge base

## The Three Object Types

### Modules

Self-contained computational units with defined inputs and outputs. A module might be a protein structure predictor, a data transformer, or an MCP server.

```yaml
type: module
module_type: compute | mcp_server | data_pipeline
inputs:
  - name: sequence
    type: protein_sequence
outputs:
  - name: structure
    type: pdb_structure
```

### Datasets

Data collections with storage, lineage, and access metadata. Datasets follow the `.dataset.yaml` schema for multi-cloud storage abstraction.

```yaml
type: dataset
dataset_class: source | derived | target
storage:
  provider: s3 | minio | local
  bucket: my-bucket
  path: /data/
```

### Lattices

Compositions of modules and datasets into executable workflows. A lattice defines how modules connect via typed edges.

```yaml
type: lattice
nodes:
  - id: predictor
    module: module_alphafold
edges:
  - source: input_data
    target: predictor
    data_mapping:
      - from: sequences
        to: input_sequences
        type: protein_sequence
```

## Type Vocabulary

19 canonical I/O types across 4 tiers:

| Tier | Types |
|------|-------|
| **Primitives** | string, number, boolean, path, json, binary |
| **Structured** | csv, dataframe, yaml_config, parameter_set |
| **Molecular** | protein_sequence, pdb_structure, sdf_molecule, msa_alignment, docking_result, md_trajectory, density_map |
| **Media** | image, canvas_json |

## FAIR Metadata

Deployed objects (modules, datasets, lattices) require FAIR metadata for findability, accessibility, interoperability, and reusability:

```yaml
fair:
  keywords: ["protein", "structure prediction"]
  license: MIT
  findability:
    identifier: doi:10.xxx
  accessibility:
    access_protocol: https
```

Minimum requirement: `keywords` + `license`.

## Content-as-Code Pipelines

Folder-based workflows where a file's location is its state. Each stage folder contains an `AGENTS.md` with processing instructions. Files move through stages as they're processed.

```
pipeline/
├── 01_inbox/       # Raw input
├── 02_processing/  # Being worked on
├── 03_review/      # Ready for review
└── 04_complete/    # Done
```

## Context Library

Curated knowledge files in `what/context/`. Organized by topic, each with an `AGENTS.md` index. Agents load relevant topics before domain-specific tasks.

## Skills

Reusable agent recipes in `how/skills/`. Two types:
- **Agent skills** — automated recipes an agent can execute
- **Process skills** — human/hybrid procedures with agent support

## Backlog

Durable ideation tracking in `how/backlog/`. Ideas graduate into missions when mature. Ideas are archived when implemented or abandoned.
