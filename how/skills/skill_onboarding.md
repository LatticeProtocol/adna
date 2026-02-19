---
type: skill
skill_type: agent
created: 2026-02-19
updated: 2026-02-19
status: active
category: onboarding
trigger: "First-run detection in CLAUDE.md indicates uncustomized vault"
last_edited_by: agent_stanley
tags: [skill, onboarding, first-run]

requirements:
  tools: []
  context: ["CLAUDE.md", "MANIFEST.md", "STATE.md"]
  permissions: ["write governance files", "create directories"]
---

# Skill: First-Run Onboarding

## Overview

Interactive onboarding flow for fresh lattice-adna vaults. Invoked automatically when CLAUDE.md first-run detection identifies an uncustomized vault. Walks the user through understanding aDNA, discovering their project needs, building an initial ontology, and customizing governance files.

## Trigger

Automatic — CLAUDE.md first-run detection triggers this skill when:
1. `how/sessions/history/` is empty (no completed sessions)
2. `MANIFEST.md` frontmatter has `last_edited_by: agent_init`

If both conditions are true, load this skill and begin at Step 1.
If only one is true (partial onboarding), resume from the first incomplete step.

## Parameters

None. This skill is self-guided through conversation with the user.

## Requirements

### Tools/APIs
- File read/write access to the vault

### Context Files
- `CLAUDE.md` — project structure and agent protocol
- `MANIFEST.md` — project identity (will be customized)
- `STATE.md` — operational state (will be customized)
- `what/docs/adna_standard.md` — full spec (reference only)

### Permissions
- Write governance files (CLAUDE.md, MANIFEST.md, STATE.md)
- Create directories and AGENTS.md files for ontology extensions

## Implementation

### Step 1: Welcome and Introduce aDNA

Greet the user warmly but directly. Introduce yourself as Berthier — the vault's built-in agent chief of staff.

Explain aDNA in 2-3 accessible sentences: it's a knowledge architecture that gives your project persistent structure both humans and AI agents can navigate. This vault was cloned from the lattice-adna starter kit and is ready to be customized for their project.

Keep it approachable — no spec-heavy language. The user might be a developer, a researcher, or a team lead. Meet them where they are.

### Step 2: Explain the Triad

Introduce the three-directory structure:

- **`what/`** = What you know — knowledge, context, decisions, domain objects
- **`how/`** = How you work — missions, sessions, templates, pipelines
- **`who/`** = Who is involved — people, teams, coordination, governance

Make it concrete with the **Question Test**: "If you have a research paper summary, that answers 'what do we know?' → `what/`. A deployment checklist answers 'how do we deploy?' → `how/`. A team roster answers 'who works on this?' → `who/`."

Mention that this is extensible — they'll add their own entity types under the right leg in Step 6.

### Step 3: Explain Lattices and Context Graphs (brief)

Lattices are directed graphs connecting datasets, modules, and reasoning nodes into executable compositions. Four types:

| Type | What it does |
|------|-------------|
| `pipeline` | Deterministic DAG of processing stages |
| `agent` | LLM-driven reasoning and decision flow |
| `context_graph` | Knowledge structure connecting concepts |
| `workflow` | Operational process for human/agent procedures |

This vault includes tools to validate and visualize lattices, plus 3 ready-to-use examples.

**Tiered depth**: Point to `what/lattices/examples/` for examples. For the full spec, see `what/docs/adna_standard.md`.

Keep this brief — 30 seconds of explanation, not a lecture.

### Step 4: Explain Deployment Diversity

Two deployment forms:
- **Bare triad** (this vault) — `what/how/who/` at project root. The full experience.
- **Embedded triad** — `.agentic/what/how/who/` inside an existing repo. For adding aDNA to existing codebases.

For multi-instance composition (nesting vaults, sibling projects), see `what/docs/adna_bridge_patterns.md`.

One or two sentences is enough here.

### Step 5: Discovery Conversation — Ask About Their Project

This is the most important step. Ask the user about their project, domain, and goals. Choose 2-3 questions that fit the conversation flow:

- "What are you building? What domain are you in?"
- "Is this for a team or are you working solo?"
- "What kind of knowledge will you be managing?" (research, code, operations, customer relationships, etc.)
- "Do you already use AI agents in your workflow?"

Listen for **domain signals** that inform ontology extensions in Step 6:
- Research terms → `what/papers/`, `what/datasets/`, `what/hypotheses/`
- Business/CRM terms → `who/customers/`, `who/partners/`, `who/contacts/`
- Software terms → `how/incidents/`, `how/deployments/`, `what/services/`
- Lab/science terms → `what/experiments/`, `what/compounds/`, `what/protocols/`
- Content/media terms → `what/publications/`, `how/editorial_pipeline/`

Don't ask all questions — read the room and ask what's relevant.

### Step 6: Suggest Ontology Extensions

Based on the discovery conversation, suggest domain-specific entity types with concrete examples. Common extensions:

| Domain | Suggested extensions |
|--------|---------------------|
| **Biotech/Science** | `what/experiments/`, `what/compounds/`, `what/protocols/`, `what/datasets/` |
| **Software team** | `how/incidents/`, `how/deployments/`, `what/services/`, `what/apis/` |
| **Research** | `what/papers/`, `what/datasets/`, `what/hypotheses/` |
| **Business/CRM** | `who/customers/`, `who/partners/`, `who/contacts/`, `who/projects/` |
| **Content/Media** | `what/publications/`, `how/editorial_pipeline/` |

For each confirmed extension:
1. Create the directory under the appropriate triad leg
2. Add an `AGENTS.md` with purpose and working rules
3. Optionally create a template in `how/templates/`

**Ask user to confirm before creating anything.** Show them the proposed structure and get approval.

### Step 7: Customize Governance Files

Update the three governance files with the user's project identity:

**MANIFEST.md:**
- Replace the project description (line 13) with the user's project name and description
- Replace the detail paragraph (lines 15-16) with project-specific details
- Update `last_edited_by` and `updated` in frontmatter

**STATE.md:**
- Update current phase to "Phase 1 — Onboarding Complete"
- Replace next steps with domain-specific actions based on discovery conversation
- Update `last_edited_by` and `updated` in frontmatter

**CLAUDE.md:**
- If the user shared a project description in Step 5, update the project description in the Identity & Personality section (first paragraph after the personality block)
- Keep Berthier personality unless they choose otherwise in Step 8
- Update `last_edited_by` and `updated` (version comment at top and bottom)

### Step 8: Personality Customization Offer

Conversational offer — don't make it a big deal:

"I'm operating as Berthier — a structured, operations-focused chief of staff. I can help you design your own agent personality for this vault, or you can keep me as-is. What would you prefer?"

**If user wants custom:**
1. Ask for: name, archetype/inspiration, 3-4 operating style principles, greeting style
2. Edit the Identity & Personality section of CLAUDE.md (between `## Identity & Personality` header and the first `---` separator)
3. Show them the result before saving

**If user keeps Berthier:**
- Confirm and move on. No changes needed.

### Step 9: Marketplace Teaser

Brief mention — 2-3 sentences max:

"One more thing — what you build here has value beyond your project. Ontology extensions, lattice definitions, skills, and templates you create can eventually be published to the **Lattice Protocol marketplace** for others to discover, deploy, and build on — with agentic residuals flowing back to you. The marketplace is coming soon. For now, just know that everything you build here is portable and composable."

This is a teaser, not a tutorial. Don't oversell.

### Step 10: Next Steps and Session Close

Summarize what was configured during onboarding.

Suggest 3-4 concrete next steps tailored to the user's domain:
- Create a context entry in `what/context/` for a key domain topic
- Start a mission in `how/missions/` for their first real task
- Create or customize a lattice definition for a workflow they described
- Add team members or governance to `who/governance/`

This session IS the onboarding session — create the session file (or update if already created), log all files touched, close with a standard SITREP.

## Outputs

| Output | Type | Description |
|--------|------|-------------|
| Customized MANIFEST.md | File | Project identity updated |
| Customized STATE.md | File | Next steps updated |
| Customized CLAUDE.md | File | Project description (and optionally personality) updated |
| Domain directories | Directories | Ontology extensions with AGENTS.md files |
| Session file | File | Onboarding session record in history |

## Error Handling

| Error | Cause | Resolution |
|-------|-------|------------|
| User quits mid-onboarding | Session interrupted | Safe to resume — Steps 1-4 are conversational (no vault changes). Steps 5-9 check existing state before modifying. |
| Governance files already customized | Partial prior onboarding | Check what's already done, skip completed steps, resume from first incomplete step |
| User declines all extensions | No domain fit yet | That's fine — the base ontology works standalone. They can extend later. |
| User declines governance edits | Wants to do it manually | Respect the choice. Summarize what they'd need to edit and where. |

## Completion Markers

After successful onboarding:
- `MANIFEST.md` will have a non-init `last_edited_by` value
- A session file will exist in `how/sessions/history/`
- Both first-run indicators cleared — future sessions skip onboarding

## Related

- **Skills Protocol**: `how/skills/AGENTS.md`
- **CLAUDE.md**: First-run detection section
- **aDNA Standard**: `what/docs/adna_standard.md`
- **Bridge Patterns**: `what/docs/adna_bridge_patterns.md`
