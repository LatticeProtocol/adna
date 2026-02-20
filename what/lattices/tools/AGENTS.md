---
type: directory_index
created: 2026-02-19
updated: 2026-02-19
last_edited_by: agent_stanley
tags: [directory_index, lattice, tools]
---

# what/lattices/tools/ — Lattice YAML Tooling

## Purpose

Python utilities for validating `.lattice.yaml` files against the JSON Schema and converting between `.lattice.yaml` and Obsidian `.canvas` formats.

## Tools

| Tool | Function | Input | Output |
|------|----------|-------|--------|
| `lattice_validate.py` | Validate `.lattice.yaml` against JSON Schema | YAML file path or parsed dict | `LatticeValidationResult` (valid/errors/warnings) |
| `lattice2canvas.py` | Convert `.lattice.yaml` → Obsidian `.canvas` | YAML file/dict | Canvas JSON file/dict |
| `canvas2lattice.py` | Convert Obsidian `.canvas` → `.lattice.yaml` | Canvas JSON file/dict | YAML file/dict |

## Dependencies

```bash
pip install -r requirements.txt  # pyyaml only
```

No additional dependencies beyond `pyyaml` and the Python standard library (`json`, `jsonschema` via validation).

## Quick Usage

### Validate a lattice file

```python
from what.lattices.tools import validate_lattice_file

result = validate_lattice_file("examples/deep_research.lattice.yaml")
print(f"Valid: {result.valid}")
for error in result.errors:
    print(f"  Error: {error}")
```

### Convert lattice to canvas

```python
from what.lattices.tools import lattice_file_to_canvas

lattice_file_to_canvas("input.lattice.yaml", "output.canvas")
```

### Convert canvas to lattice

```python
from what.lattices.tools import canvas_file_to_lattice

canvas_file_to_lattice("input.canvas", "output.lattice.yaml")
```

## Load/Skip Decision

**Load this directory when**:
- Validating a new or modified `.lattice.yaml` file against the schema
- Converting between `.lattice.yaml` and Obsidian `.canvas` formats
- Debugging lattice validation errors or extending the tooling

**Skip when**:
- Working on non-lattice tasks (sessions, context, governance, CRM)
- Reading or authoring lattice YAML manually without needing programmatic validation

**Token cost**: ~400 tokens (this AGENTS.md). Individual Python files are ~200-400 lines each.

## Cross-References

- [what/lattices/AGENTS](../AGENTS.md) — Lattice YAML ecosystem overview
- [what/lattices/examples/AGENTS](../examples/AGENTS.md) — Reference lattice implementations
- `../lattice_yaml_schema.json` — JSON Schema used by the validator
- `../canvas_yaml_interop.md` — Bidirectional mapping specification
