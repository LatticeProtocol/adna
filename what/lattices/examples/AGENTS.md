---
type: directory_index
created: 2026-02-19
updated: 2026-02-19
last_edited_by: agent_stanley
tags: [directory_index, lattice, examples]
---

# what/lattices/examples/ — Reference Lattice Implementations

## Purpose

Example `.lattice.yaml` files demonstrating different lattice types, execution modes, and composition patterns. Use as starting templates when creating new lattices.

## Examples

| File | Lattice Type | Execution Mode | Demonstrates |
|------|-------------|---------------|-------------|
| `deep_research.lattice.yaml` | pipeline | hybrid | Multi-agent research pipeline with validation loops, phase gates, mixed workflow/reasoning nodes |
| `research_orchestrator.lattice.yaml` | agent | hybrid | Orchestrator pattern — LLM-driven coordination of sub-tasks with human checkpoints |
| `protein_binder_design.lattice.yaml` | pipeline | workflow | Deterministic computational biology pipeline — linear DAG, no reasoning nodes |

## How to Use

1. **Pick the closest example** to your target lattice type and execution mode
2. **Copy** the file with a new name: `your_lattice_name.lattice.yaml`
3. **Modify** nodes, edges, metadata, and FAIR block to match your design
4. **Validate** with `tools/lattice_validate.py` to check schema compliance

## Load/Skip Decision

**Load this directory when**:
- Creating a new `.lattice.yaml` file and need a structural template
- Learning the lattice YAML format by example
- Comparing execution modes (workflow vs. reasoning vs. hybrid)

**Skip when**:
- Already familiar with the `.lattice.yaml` format and not creating new lattices
- Working on non-lattice tasks (sessions, context, governance)

**Token cost**: ~250 tokens (this AGENTS.md). Individual YAML files are ~500-1,000 tokens each.

## Cross-References

- [what/lattices/AGENTS](../AGENTS.md) — Lattice YAML ecosystem overview
- [what/lattices/tools/AGENTS](../tools/AGENTS.md) — Validation and conversion tools
- `../lattice_yaml_schema.json` — JSON Schema for validation
- `../canvas_yaml_interop.md` — Canvas ↔ YAML bidirectional mapping
