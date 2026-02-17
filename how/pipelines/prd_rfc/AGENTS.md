---
type: directory_index
created: 2026-02-17
updated: 2026-02-17
last_edited_by: agent_init
tags: [directory_index, pipeline, prd_rfc]
---

# PRD/RFC Pipeline

A 4-stage content-as-code pipeline for product planning. Files flow through research, requirements, design, and review stages — producing approved PRD + RFC document pairs.

## Pipeline Flow

```
01_research/ → 02_requirements/ → 03_design/ → 04_review/ → how/deliverables/
(problem)      (PRD)              (RFC)         (approval)   (committed)
```

## Stages

| Stage | Directory | Output | Gate |
|-------|-----------|--------|------|
| 1. Research | `01_research/` | Structured research findings | Quality: 4 dimensions covered |
| 2. Requirements | `02_requirements/` | PRD document | **Human gate**: PRD approval required |
| 3. Design | `03_design/` | RFC document | Quality: traceability + alternatives |
| 4. Review | `04_review/` | Approved PRD+RFC | **Human gate**: final approval |

## How to Use

### Starting a New Project

1. Create a markdown file describing the problem/feature/idea
2. Drop it into `01_research/`
3. Follow the `01_research/AGENTS.md` instructions

### Pipeline Frontmatter

Files in the pipeline carry additional frontmatter:

```yaml
---
pipeline: prd_rfc
stage: research | requirements | design | review
project_slug: <identifier>
project_title: "Human-readable title"
priority: 1 | 2 | 3
source_type: backlog_idea | feature_request | problem_statement
source_ref: "path/to/source.md"
initiated: YYYY-MM-DD
---
```

### Completing the Pipeline

When a PRD+RFC package passes final review:
1. Pipeline frontmatter is stripped
2. PRD commits to `how/deliverables/prd_<project_slug>.md`
3. RFC commits to `how/deliverables/rfc_<project_slug>.md`
4. Optionally, a mission is created in `how/missions/`

## Campaign Planning Mode

Campaigns (multi-mission initiatives) may use the R&D→PRD→RFC lifecycle for their planning phase:

1. **Research**: Problem space exploration and opportunity assessment
2. **PRD**: Requirements for the campaign's deliverables
3. **RFC**: Technical design for the campaign's implementation

A campaign with 5+ missions benefits from formal R&D→PRD→RFC planning. Smaller campaigns (2-4 missions) may skip the R&D artifact and go straight to PRD→RFC.

## References

- Stage instructions: `01_research/AGENTS.md`, `02_requirements/AGENTS.md`, `03_design/AGENTS.md`, `04_review/AGENTS.md`
- Pipeline paradigm: [[how/pipelines/AGENTS|how/pipelines/AGENTS.md]]
- Templates: [[template_prd|PRD]], [[template_rfc|RFC]]
- Output destination: `how/deliverables/`
- Mission protocol: [[how/missions/AGENTS|how/missions/AGENTS.md]]
- Campaign protocol: [[how/campaigns/AGENTS|how/campaigns/AGENTS.md]]
