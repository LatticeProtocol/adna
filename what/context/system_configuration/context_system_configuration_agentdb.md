---
type: context_guide
topic: system_configuration
subtopic: agentdb
created: 2026-03-27
updated: 2026-03-27
sources: ["AgentDB implementation patterns", "SQLite best practices", "aDNA operational protocols"]
context_version: "1.0"
token_estimate: ~2500
last_edited_by: agent_aria
tags: [context, system_configuration, agentdb, sqlite, operational_memory, persistence]
---

# System Configuration: AgentDB

SQLite-based operational memory for AI agents. Stores cross-session state — learnings, errors, contracts, checkpoints — at `_meta/agentdb/agent.db` per project.

---

## Key Principles

| # | Principle | Rule |
|---|-----------|------|
| 1 | **SQLite as agent memory** | AgentDB uses SQLite databases to store cross-session operational state: learnings, errors, contracts, checkpoints, and context. SQLite is chosen for zero-dependency, file-based, concurrent-read access. No server, no setup, no network dependency. |
| 2 | **Absolute paths always** | NEVER rely on `$PWD` to resolve AgentDB paths. Sub-repos with their own `_meta/` will silently route to the wrong database. Always use the full absolute path to the target `agent.db`. This is the #1 source of AgentDB bugs. |
| 3 | **Multi-level isolation** | Each project gets its own `agent.db`. Vault-level databases aggregate cross-project patterns. Workspace-level databases (optional) provide global state. Data flows upward (project to vault to workspace) via sync, never downward implicitly. |
| 4 | **Learnings as first-class data** | The `learnings` table captures patterns (things that worked), failures (things that broke), and gotchas (surprising behaviors). Each learning has evidence, a timestamp, and a reinforcement count. Unreinforced learnings decay and should be pruned. |
| 5 | **Session lifecycle integration** | Agents read AgentDB at session start (load active contracts, recent errors, relevant patterns) and write at session end (checkpoint state, record learnings, close contracts). No session should end without a write. |
| 6 | **Schema versioning** | AgentDB schemas evolve. Migrations are tracked (e.g., `004_fix_learnings_schema`). Always check for pending migrations at session start. Apply migrations before querying. Back up the `.db` file before altering tables. |
| 7 | **Contracts for multi-agent work** | When orchestrating tier 2+ work, the orchestrator writes a contract (goal, files, tier, success criteria) to AgentDB. Surgeon agents read the contract, execute, and checkpoint progress back to the database. |

---

## Database Layout

Multi-level isolation in practice:

```
~/lattice/
├── _meta/agentdb/
│   └── agent.db              # Workspace-level (cross-vault patterns)
├── coding_vault/
│   ├── _meta/agentdb/
│   │   └── agent.db          # Vault-level (cross-project patterns)
│   └── project_alpha/
│       └── _meta/agentdb/
│           └── agent.db      # Project-level (project-specific state)
```

| Level | Scope | Contains |
|-------|-------|----------|
| **Project** | Single repo/project | Session state, project-specific learnings, contracts, errors |
| **Vault** | All projects in a vault | Cross-project patterns, aggregated failure modes |
| **Workspace** | All vaults | Global patterns, routing decisions, cost tracking |

Data flows **upward only**. A project learning promoted to vault level is copied, not moved. Workspace state never overwrites project state.

---

## Core Schema

Minimal tables for a functional AgentDB:

```sql
CREATE TABLE IF NOT EXISTS learnings (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  type TEXT CHECK(type IN ('pattern', 'failure', 'gotcha')) NOT NULL,
  what TEXT NOT NULL,
  evidence TEXT,
  reinforcement_count INTEGER DEFAULT 0,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  last_reinforced DATETIME
);

CREATE TABLE IF NOT EXISTS context_sessions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  agent TEXT NOT NULL,
  event TEXT NOT NULL,
  branch TEXT,
  data JSON,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS errors (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  tool TEXT,
  stderr TEXT,
  context TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS contracts (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  goal TEXT NOT NULL,
  files JSON,
  tier INTEGER,
  status TEXT CHECK(status IN ('active', 'completed', 'blocked')) DEFAULT 'active',
  success_criteria TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  completed_at DATETIME
);

CREATE TABLE IF NOT EXISTS checkpoints (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  contract_id INTEGER REFERENCES contracts(id),
  agent TEXT,
  commit_hash TEXT,
  files_touched JSON,
  evidence TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

| Table | Purpose | Key Fields |
|-------|---------|------------|
| `learnings` | Cross-session knowledge | `type` (pattern/failure/gotcha), `reinforcement_count` |
| `context_sessions` | Session lifecycle tracking | `agent`, `event` (start/end/checkpoint), `data` (JSON) |
| `errors` | Tool and execution failures | `tool`, `stderr`, `context` |
| `contracts` | Multi-agent task coordination | `goal`, `files`, `tier`, `status` |
| `checkpoints` | Progress snapshots per contract | `contract_id`, `commit_hash`, `evidence` |

---

## Session Lifecycle

| Phase | Action | Command |
|-------|--------|---------|
| **Start** | Read recent failures, active contracts, relevant patterns | `agentdb read-start` |
| **During** | Record errors as they occur; checkpoint after working states | `agentdb checkpoint '{"commit":"abc123","files":["x.ts"]}'` |
| **End** | Write learnings, close contracts, record session summary | `agentdb write-end '{"did":"X","learned":["Y"]}'` |

### Start Queries

```sql
-- Recent failures (last 7 days)
SELECT * FROM errors WHERE created_at > datetime('now', '-7 days') ORDER BY created_at DESC;

-- Active contracts
SELECT * FROM contracts WHERE status = 'active';

-- High-value learnings (reinforced 2+ times)
SELECT * FROM learnings WHERE reinforcement_count >= 2 ORDER BY last_reinforced DESC;
```

### End Writes

```sql
-- Record a learning
INSERT INTO learnings (type, what, evidence) VALUES ('pattern', 'WAL mode prevents locking', '3-agent parallel session');

-- Reinforce an existing learning
UPDATE learnings SET reinforcement_count = reinforcement_count + 1, last_reinforced = CURRENT_TIMESTAMP WHERE id = 42;

-- Close a contract
UPDATE contracts SET status = 'completed', completed_at = CURRENT_TIMESTAMP WHERE id = 7;
```

---

## Contract Workflow

For orchestrator-surgeon multi-agent patterns:

| Step | Actor | Action |
|------|-------|--------|
| 1 | Orchestrator | Writes contract to AgentDB: goal, files, tier, success criteria |
| 2 | Surgeon | Reads contract at session start |
| 3 | Surgeon | Executes, checkpoints progress after each working state |
| 4 | Surgeon | Marks contract completed with evidence (commit hash, test output) |
| 5 | Orchestrator | Reads checkpoint, verifies, proceeds or reassigns |

```bash
# Orchestrator writes
agentdb contract '{"goal":"Add auth middleware","files":["src/auth.ts","src/middleware.ts"],"tier":2}'

# Surgeon reads
agentdb query "SELECT * FROM contracts WHERE status='active' ORDER BY created_at DESC LIMIT 1"

# Surgeon checkpoints
agentdb checkpoint '{"contract_id":1,"commit":"a1b2c3d","files":["src/auth.ts"],"evidence":"tests pass"}'
```

---

## Recommendations

| Area | Recommendation |
|------|---------------|
| **Database size** | Keep under 500KB. AgentDB is operational memory, not a data warehouse. Archive old learnings. |
| **WAL mode** | Enable for concurrent access. Multiple agents can read simultaneously without locks. `PRAGMA journal_mode=WAL;` |
| **Pruning** | Learnings never reinforced after 30+ days are noise. Run `agentdb prune` periodically. Target: under 200 active learnings. |
| **Backups** | Copy the `.db` file before schema migrations. SQLite files are portable — `cp agent.db agent.db.bak`. |
| **Migrations** | Track with numbered prefixes (e.g., `004_fix_learnings_schema`). Check for pending migrations at session start. |
| **JSON fields** | Use SQLite JSON functions (`json_extract`, `json_each`) for querying `data` and `files` columns. |
| **Indexing** | Add indexes on `created_at` and `status` columns once tables exceed 1000 rows. |

---

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| **Relative paths to agent.db** | Sub-repo `_meta/` shadows parent — writes go to wrong database | Always use absolute paths |
| **Never pruning** | 100+ unreinforced learnings dilute signal | Prune monthly; delete learnings with 0 reinforcements older than 30 days |
| **Skipping session-end writes** | Lost learnings, broken contract chains, no checkpoint for handoff | Make write-end mandatory in session protocol |
| **Storing large blobs** | Database bloat, slow queries, defeats operational-memory purpose | Store references (file paths, commit hashes), not contents |
| **No migration tracking** | Schema changes without versioning corrupt existing databases | Number migrations, check version before queries |
| **Single global database** | Project failures pollute another project's context | One `agent.db` per project; aggregate upward explicitly |
| **Reading without filtering** | Loading all learnings wastes tokens on stale data | Filter by recency (`-7 days`), reinforcement count (`>= 2`), or type |
| **Downward data flow** | Workspace-level patterns overwrite project-specific state | Data flows up only; project databases are authoritative for their scope |

---

## SQLite Pragmas

Recommended pragmas for AgentDB databases:

```sql
PRAGMA journal_mode = WAL;          -- Concurrent reads without locking
PRAGMA busy_timeout = 5000;         -- Wait 5s on lock instead of failing
PRAGMA synchronous = NORMAL;        -- Balance durability and performance
PRAGMA foreign_keys = ON;           -- Enforce contract-checkpoint relationships
```

---

## Sources

- AgentDB implementation patterns — SQLite operational memory for AI agents
- SQLite best practices — WAL mode, concurrent access, file-based databases
- aDNA operational protocols — session lifecycle, `_meta/` conventions
