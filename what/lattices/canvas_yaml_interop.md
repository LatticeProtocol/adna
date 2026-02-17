---
type: specification
created: 2026-02-16
updated: 2026-02-17
status: active
last_edited_by: agent_init
tags: [specification, lattice, canvas, interop, schema]
---

# Canvas-YAML Interop Specification

Bidirectional mapping between `.lattice.yaml` and Obsidian `.canvas` JSON format. Enables visual editing of lattice graphs in Obsidian with round-trip conversion to the machine-readable YAML schema.

## Purpose

Obsidian Canvas provides a visual node-graph editor. Lattice YAML provides a machine-readable, version-controlled definition. This spec defines:
1. How to **convert** a `.lattice.yaml` → `.canvas` for visual editing
2. How to **convert** a `.canvas` → `.lattice.yaml` for execution
3. What is **preserved** and what is **lost** in each direction
4. **Color conventions** for visual consistency

## Reference Formats

### Obsidian Canvas JSON Structure

```json
{
  "nodes": [
    {
      "id": "unique_id",
      "type": "file" | "text" | "group" | "link",
      "file": "vault/path/to/file.md",
      "text": "Markdown content",
      "x": 0, "y": 0,
      "width": 350, "height": 300,
      "color": "1"
    }
  ],
  "edges": [
    {
      "id": "edge_id",
      "fromNode": "source_id",
      "fromSide": "bottom" | "top" | "left" | "right",
      "toNode": "target_id",
      "toSide": "top" | "bottom" | "left" | "right",
      "label": "Edge Label"
    }
  ]
}
```

### Lattice YAML Structure

Defined by: `what/lattices/lattice_yaml_schema.json`

```yaml
lattice:
  name: example
  version: "1.0.0"
  lattice_type: pipeline
  description: "..."
  execution: { mode: workflow, runtime: local, tier: L1 }
  nodes:
    - id: node_id
      type: module | dataset | process | reasoning
      ref: "what/modules/module_name"
      config: {}
  edges:
    - from: source_id
      to: target_id
      label: "data flow"
      data_mapping: {}
      condition: ""
  fair: { license: MIT, keywords: [...] }
```

## Node Mapping

### YAML → Canvas

| YAML `nodes[].type` | Canvas `type` | Canvas `file` / `text` | Notes |
|---------------------|---------------|------------------------|-------|
| `module` | `file` | `ref` value (vault path + `.md`) | Links to module registry entry |
| `dataset` | `file` | `ref` value (vault path + `.md`) | Links to dataset registry entry |
| `process` | `text` | `description` or `id` as markdown | No vault file to link |
| `reasoning` | `text` | `prompt` as markdown, prefixed with `## {id}` | Prompt shown as card content |

**Canvas node fields derived from YAML**:
- `id` → canvas `id` (preserved 1:1)
- `description` → appended to canvas `text` for text nodes
- `config` → not represented in canvas (YAML-only)

**Canvas node fields auto-generated**:
- `x`, `y` — auto-layout (left-to-right DAG, 400px horizontal spacing)
- `width` — 350 (default)
- `height` — 200 (text) or 300 (file)

### Canvas → YAML

| Canvas `type` | YAML `nodes[].type` | Inference Rule |
|---------------|---------------------|----------------|
| `file` with path in `what/modules/` | `module` | Path prefix match |
| `file` with path in `what/datasets/` | `dataset` | Path prefix match |
| `file` with other path | `module` (default) | Fallback — may need manual correction |
| `text` | `process` | Default for text nodes |
| `text` with `## Reasoning:` prefix | `reasoning` | Convention: prefix signals reasoning node |
| `group` | (ignored) | Groups are canvas-only layout containers |
| `link` | (ignored) | External links not part of lattice graph |

**YAML fields derived from canvas**:
- canvas `id` → YAML `id`
- canvas `file` → YAML `ref` (strip `.md` extension)
- canvas `text` → YAML `description` (for process) or `prompt` (for reasoning)

## Edge Mapping

### YAML → Canvas

| YAML Edge Field | Canvas Edge Field | Notes |
|----------------|-------------------|-------|
| `from` | `fromNode` | Direct mapping |
| `to` | `toNode` | Direct mapping |
| `label` | `label` | Direct mapping |
| `port` | — | **YAML-only** — not representable in canvas |
| `data_mapping` | — | **YAML-only** — not representable in canvas |
| `condition` | `label` (appended) | Condition shown as `[label] (if: condition)` |

**Canvas edge fields auto-generated**:
- `id` — generated as `{from}_to_{to}`
- `fromSide` — `right` (default for left-to-right layout)
- `toSide` — `left` (default for left-to-right layout)

### Canvas → YAML

| Canvas Edge Field | YAML Edge Field | Notes |
|-------------------|----------------|-------|
| `fromNode` | `from` | Direct mapping |
| `toNode` | `to` | Direct mapping |
| `label` | `label` | Direct mapping (condition suffix stripped if present) |
| `fromSide` | — | **Canvas-only** — layout information |
| `toSide` | — | **Canvas-only** — layout information |
| `fromFloating` | — | **Canvas-only** |
| `toFloating` | — | **Canvas-only** |

## Color Conventions

Obsidian Canvas supports colors 1-6 as string values. Lattice uses these conventions for visual consistency:

| Color Code | Color | Lattice Node Type | Mnemonic |
|-----------|-------|-------------------|----------|
| `"1"` | Red | — (reserved for warnings/errors) | Alert |
| `"2"` | Orange | — (reserved for annotations/notes) | Note |
| `"3"` | Yellow | — (reserved for highlights) | Highlight |
| `"4"` | Cyan | `module` | Module = building block |
| `"5"` | Green | `dataset` | Dataset = data |
| `"6"` | Purple | `reasoning` | Reasoning = AI |
| (none) | Default | `process` | Process = generic step |

## Root-Level Metadata

### YAML → Canvas

The root-level `lattice` fields (`name`, `version`, `lattice_type`, `description`, `execution`, `fair`) have no canvas equivalent. They are stored in a companion sidecar or embedded as a canvas group node:

**Strategy**: Create a `group` node at top-left containing a `text` node with the lattice metadata rendered as markdown:

```json
{
  "id": "_lattice_meta",
  "type": "group",
  "label": "protein_binder_design v1.0.0",
  "x": -200, "y": -100,
  "width": 300, "height": 200
}
```

### Canvas → YAML

The `_lattice_meta` group node is parsed back into root-level fields. If absent, the converter prompts for required fields (`name`, `version`, `description`, `execution`, `fair`).

## Round-Trip Guarantees

### YAML → Canvas → YAML

| Category | Preserved | Lost / Degraded |
|----------|-----------|-----------------|
| Node IDs | Yes | — |
| Node types | Yes (via color + path inference) | May need manual correction for ambiguous `file` nodes |
| Node `ref` | Yes (from canvas `file` field) | — |
| Node `config` | **No** — YAML-only | Must be restored from original YAML |
| Node `prompt` | Yes (from canvas `text`) | Formatting may degrade |
| Node `model_override` | **No** — YAML-only | Must be restored from original YAML |
| Edge `from`/`to` | Yes | — |
| Edge `label` | Yes | — |
| Edge `port` | **No** — YAML-only | Must be restored from original YAML |
| Edge `data_mapping` | **No** — YAML-only | Must be restored from original YAML |
| Edge `condition` | Partial (in label suffix) | Formal expression lost; best-effort parse |
| `execution` block | **No** — stored in group | Must be restored from original YAML |
| `fair` block | **No** — stored in group | Must be restored from original YAML |

**Recommendation**: When round-tripping, use the original YAML as the source of truth for YAML-only fields. Canvas edits should only modify graph topology (add/remove/reconnect nodes and edges) and node descriptions.

### Canvas → YAML → Canvas

| Category | Preserved | Lost / Degraded |
|----------|-----------|-----------------|
| Node IDs | Yes | — |
| Node positions (x, y) | **No** — YAML has no layout | Auto-layout on re-import |
| Node dimensions | **No** — YAML has no layout | Default sizes applied |
| Node colors | Yes (via type→color convention) | Custom colors lost |
| Edge routing (`fromSide`/`toSide`) | **No** — YAML has no layout | Default routing applied |
| Edge `fromFloating`/`toFloating` | **No** — canvas-only | Lost |
| Group nodes | **No** — canvas-only (except `_lattice_meta`) | Lost |
| Link nodes | **No** — canvas-only | Lost |

**Recommendation**: Canvas→YAML is a one-way export for initial lattice creation. For ongoing editing, prefer YAML as source of truth with Canvas as read-only visualization.

## Conversion Workflow

### Creating a New Lattice (Visual-First)

1. Create `.canvas` file in Obsidian
2. Add module/dataset nodes by dragging vault files onto canvas
3. Add process/reasoning nodes as text cards
4. Connect with edges, add labels
5. Run `canvas_to_yaml` converter → produces `.lattice.yaml`
6. Fill in YAML-only fields (`execution`, `fair`, `config`, `data_mapping`)
7. Validate against `lattice_yaml_schema.json`

### Visualizing an Existing Lattice (YAML-First)

1. Run `yaml_to_canvas` converter → produces `.canvas`
2. Open in Obsidian for visual review
3. Edit topology in canvas if needed
4. Run `canvas_to_yaml` to capture topology changes
5. Merge with original YAML to preserve YAML-only fields

### Keeping in Sync

The YAML file is the **source of truth**. Canvas files are derived artifacts for visualization. The recommended workflow:

1. Edit `.lattice.yaml` for all semantic changes
2. Regenerate `.canvas` from YAML when visual review is needed
3. Only use Canvas→YAML when topology changes are easier to make visually

## Cross-References

- [[what/lattices/lattice_yaml_schema.json|Lattice YAML Schema]] — JSON Schema that YAML files validate against
- [[what/lattices/AGENTS|Lattices AGENTS.md]] — Directory index
