---
type: context_core
topic: object_standards
subtopic: overview
created: 2026-03-05
updated: 2026-03-18
sources: ["aDNA Standard v2.1 (what/docs/adna_standard.md)", "lattice_yaml_schema.json", "dataset_yaml_schema.json", "what/lattices/examples/ (13 examples)", "adna_core type_vocabulary context", "adna_core fair_mapping context"]
context_version: "2.0"
token_estimate: ~1000
last_edited_by: agent_stanley
tags: [context, object_standards]
quality_score: 3.8
signal_density: 4
actionability: 4
coverage_uniformity: 3
source_diversity: 3
cross_topic_coherence: 4
freshness_category: stable
last_evaluated: 2026-03-18
---

# Object Standards: Overview

Standards, templates, and FAIR metadata requirements for the three core aDNA object types.

## Three Core Object Types

| Object | Directory | Template | Schema | Examples |
|--------|-----------|----------|--------|----------|
| Module | `what/modules/` | — | — | See vault modules |
| Dataset | `what/datasets/` | — | `dataset_yaml_schema.json` | Targets in `what/datasets/targets/` |
| Lattice | `what/lattices/` | — | `lattice_yaml_schema.json` | 13 examples in `what/lattices/examples/` |

**Targets** are a dataset subtype with `dataset_class: target`. They model biological or research targets.

Normative specification: `what/docs/adna_standard.md` (aDNA Standard v2.1, RFC 2119 keywords).

## Module Standard

Modules are self-contained computational units — one function, one model, one tool.

### Required Frontmatter

```yaml
type: module
module_type: compute         # compute | mcp_server | data_pipeline | utility
status: active               # active | deprecated | planned
inputs:
  - name: sequence
    type: protein_sequence   # must use canonical type vocabulary
    description: "Amino acid sequence to fold"
outputs:
  - name: structure
    type: pdb_structure
    description: "Predicted 3D structure"
fair:
  keywords: ["protein", "structure prediction"]
  license: MIT
```

### Quality Criteria

1. **Single responsibility** — one module, one purpose
2. **Typed I/O** — all inputs and outputs use the 19-type canonical vocabulary
3. **Explicit dependencies** — listed in frontmatter or body
4. **Version tracked** — `version` field in frontmatter
5. **FAIR annotated** — minimum `keywords` + `license`

Reference: aDNA Standard v2.1 §4.1.

## Dataset Standard

Datasets describe data collections with storage abstraction, lineage, and access metadata.

### Required Frontmatter

```yaml
type: dataset
dataset_class: source        # source | derived | target | benchmark
status: active
storage:
  provider: local            # s3 | minio | gcs | azure | ceph | local | fuse
  bucket: data-bucket        # for cloud providers
  path: /data/sequences/
  format: fasta              # csv | parquet | pdb | fasta | sdf | json
lineage:
  source: "UniProt release 2026_01"
  transformations:
    - "Filtered to human proteins"
    - "Length > 50 residues"
fair:
  keywords: ["protein sequences", "human proteome"]
  license: "CC-BY-4.0"
```

### `.dataset.yaml` Schema

For deployed datasets, the `.dataset.yaml` file provides machine-readable metadata:
- Multi-cloud storage abstraction (7 providers: S3, MinIO, GCS, Azure, Ceph, local, FUSE)
- Lineage tracking with transformation history
- Federation metadata for cross-node data sharing
- Validated against `what/datasets/dataset_yaml_schema.json`

Reference: aDNA Standard v2.1 §4.2.

## Lattice Standard

Lattices compose modules and datasets into executable workflows.

### Required Structure

```yaml
# Based on hello_world.lattice.yaml (what/lattices/examples/)
lattice:
  name: my_pipeline
  version: "1.0.0"
  lattice_type: pipeline     # pipeline | agent | context_graph | workflow
  description: "What this lattice does"
  execution:
    mode: workflow            # workflow | reasoning | hybrid
    runtime: local            # local | ray | kubernetes
    tier: L1                  # L1 (edge) | L2 (regional) | L3 (cloud)
  nodes:
    - id: input_data
      type: dataset           # dataset | module | process | reasoning
      description: "Input description"
    - id: analyze
      type: module
      module: module_name     # references a module record
      config: {}
  edges:
    - from: input_data
      to: analyze
      label: "raw data"
      data_mapping:
        - from: output_field
          to: input_field
          type: csv           # type vocabulary type
  fair:
    license: "MIT"
    keywords: ["domain", "tags"]
```

### Design Principles

1. **Typed edges** — connections specify data types from the 19-type vocabulary
2. **Composability** — lattices reference other lattices as sub-lattices
3. **Reproducibility** — full configuration captured for deterministic replay
4. **Visualization** — Canvas JSON (`.canvas`) is the visual serialization via round-trip tools

### Shipped Examples

13 examples in `what/lattices/examples/` across domains:
- **Business**: `sales_pipeline`, `product_launch`, `creative_brief`
- **Research**: `deep_research`, `knowledge_base`, `research_orchestrator`, `learning_path`
- **Biotech**: `protein_binder_design`, `binder_generation`, `composed_therapeutics`, `docking_assessment`, `full_therapeutics`
- **Onboarding**: `hello_world` (start here)

Reference: aDNA Standard v2.1 §4.3. Schema: `what/lattices/lattice_yaml_schema.json`.

## FAIR Metadata

All deployed objects require FAIR (Findable, Accessible, Interoperable, Reusable) metadata.

### Minimum Requirement

```yaml
fair:
  keywords: ["protein", "binder design"]
  license: MIT
```

### Full Envelope (Nested Form)

```yaml
fair:
  keywords: ["protein", "binder design"]
  license: MIT
  findability:
    identifier: doi:10.xxx
    metadata_catalog: internal
  accessibility:
    access_protocol: https
    authentication: required
  interoperability:
    standard: "aDNA v2.1"
    format: yaml
  reusability:
    provenance: "Generated by pipeline X"
    usage_license: MIT
```

### Flat Form (Transport)

For wire/transport contexts, FAIR metadata flattens to prefixed keys:

```yaml
fair_keywords: ["protein", "binder design"]
fair_license: MIT
fair_findability_identifier: doi:10.xxx
```

Interconversion rules: `what/context/adna_core/context_adna_core_fair_mapping.md`.

## Type Vocabulary Quick Reference

| Tier | Types |
|------|-------|
| Primitives | `string`, `number`, `boolean`, `path`, `json`, `binary` |
| Structured | `csv`, `dataframe`, `yaml_config`, `parameter_set` |
| Molecular | `protein_sequence`, `pdb_structure`, `sdf_molecule`, `msa_alignment`, `docking_result`, `md_trajectory`, `density_map` |
| Media | `image`, `canvas_json` |

Full reference: `what/context/adna_core/context_adna_core_type_vocabulary.md`.

## Sources

- aDNA Standard v2.1 (`what/docs/adna_standard.md`) — normative specification
- Lattice YAML schema (`what/lattices/lattice_yaml_schema.json`)
- Dataset YAML schema (`what/datasets/dataset_yaml_schema.json`)
- 13 example lattices (`what/lattices/examples/`)
- Type vocabulary context (`what/context/adna_core/context_adna_core_type_vocabulary.md`)
- FAIR mapping context (`what/context/adna_core/context_adna_core_fair_mapping.md`)
