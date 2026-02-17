---
type: directory_index
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [directory_index, lattice]
---

# what/lattices/ — Lattice YAML Definitions

## Purpose

This directory contains the Lattice YAML ecosystem: schema definitions, conversion tools, interop specifications, and example lattice files.

## Contents

| Path | Purpose |
|------|---------|
| `lattice_yaml_schema.json` | JSON Schema for `.lattice.yaml` validation |
| `canvas_yaml_interop.md` | Bidirectional mapping spec between `.lattice.yaml` and Obsidian `.canvas` |
| `tools/` | Python tools for validation and conversion |
| `examples/` | Example `.lattice.yaml` files |

## Tools

| Tool | Function | Usage |
|------|----------|-------|
| `tools/lattice_validate.py` | Validate `.lattice.yaml` against schema | `validate_lattice(data)` or `validate_lattice_file(path)` |
| `tools/lattice2canvas.py` | Convert `.lattice.yaml` → Obsidian `.canvas` | `lattice_to_canvas(data)` or `lattice_file_to_canvas(yaml_path, canvas_path)` |
| `tools/canvas2lattice.py` | Convert `.canvas` → `.lattice.yaml` | `canvas_to_lattice(data)` or `canvas_file_to_lattice(canvas_path, yaml_path)` |

### Installation

```bash
pip install -r tools/requirements.txt  # pyyaml only
```

### Quick Start

```python
from what.lattices.tools import validate_lattice_file, LatticeValidationResult

result = validate_lattice_file("examples/deep_research.lattice.yaml")
print(f"Valid: {result.valid}")
for error in result.errors:
    print(f"  Error: {error}")
```

## Examples

| File | Type | Description |
|------|------|-------------|
| `examples/deep_research.lattice.yaml` | pipeline (hybrid) | Multi-agent research pipeline with validation loops |
| `examples/research_orchestrator.lattice.yaml` | agent (hybrid) | Orchestrator for deep research process |
| `examples/protein_binder_design.lattice.yaml` | pipeline (workflow) | Computational protein design pipeline |

## Lattice Types

| Type | Description | Mode |
|------|-------------|------|
| `pipeline` | Deterministic DAG of modules | `workflow` |
| `agent` | LLM-driven reasoning | `reasoning` |
| `context_graph` | Knowledge structure | varies |
| `workflow` | Operational process | `workflow` |

## Cross-References

- [what/AGENTS](what/AGENTS.md) — Knowledge layer index
- [canvas_yaml_interop](what/lattices/canvas_yaml_interop.md) — Interop specification
