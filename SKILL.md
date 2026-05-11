---
name: mesh
description: >
  Self-Authoring Agent Mesh — detects your connected MCPs, asks what to watch,
  then auto-creates agents that observe patterns and evolve through natural
  selection. Works with ANY MCP combination. Use when user says "mesh",
  "run mesh", "observe", "spawn agents", "mesh status", "prune", or runs /mesh.
---

# Self-Authoring Agent Mesh

## Token Efficiency Rules (CRITICAL)

1. **Never read agent specs at runtime** — behavior is defined HERE. Vault specs exist for humans and the Pruner only.
2. **Rolling observations** — max 3 files. Before writing a 4th, compress oldest's recurring/chronic patterns into `System/digest.md` (one line each), then delete it.
3. **Logs overwrite** — one file per agent (`System/Logs/<name>.md`), overwritten each run. Header tracks `run_count` and `last_5_dates`.
4. **Registry is the only file read every run** — keep under 50 lines.
5. **Compact formats** — patterns are 3 lines max. Agent specs ≤30 lines.
6. **Surgical MCP queries** — use filters, limits, date ranges. Max 3 calls per MCP. Parallel when possible.

## Pre-flight

1. Check for vault access (vault MCP or local `./claude.md`)
2. Discover ALL available MCP tools — list every tool prefix/namespace available in the current session
3. Need vault + at least 1 non-vault MCP. Stop with message if missing.

## Routing

| Intent | Action |
|---|---|
| "mesh", "set up mesh", "bootstrap" | [Bootstrap](#bootstrap) |
| "observe", "scan" | [Observer](#observer) |
| "architect", "spawn" | [Architect](#architect) |
| "run [agent-name]" | [Execute](#execute) |
| "prune", "evaluate" | [Pruner](#pruner) |
| "mesh status" | [Status](#status) |
| "mesh cycle", "full cycle" | Observer → Architect → Execute all active agents |

## Vault Structure

```
System/
  registry.md       — Agent index (ONLY file read every run)
  digest.md         — Compressed pattern history
  Agents/           — Human-readable specs (NOT read at runtime)
  Observations/     — Rolling window of 3 max
  Logs/             — One file per agent (overwritten each run)
```

## Bootstrap

### Step 1: Discover MCPs

Scan all available tools in the session. Group them by MCP namespace/prefix. Present what was found:

```
I found these connected systems:

1. Slack — messaging, channels, search
2. HubSpot — CRM, deals, contacts, companies
3. GitHub — repos, PRs, issues
4. [any others discovered]

Which of these should the mesh watch? (numbers, names, or 'all')
```

The list is dynamic — it comes from whatever MCPs are actually available, not a hardcoded list. For each MCP, briefly describe what it provides based on the tool names discovered.

### Step 2: Understand each selected MCP

For EACH MCP the user selected, ask ONE targeted question:

> "For [MCP name] — what matters most to you? What patterns, problems, or manual work should I watch for? (or 'everything' to auto-discover)"

Do this as a SINGLE message listing all selected MCPs, not one-by-one:

```
Quick questions so I know what to watch:

1. **Slack**: What matters most? (e.g., unanswered questions, manual reporting, specific channels — or 'everything')
2. **HubSpot**: What matters most? (e.g., stale deals, missing data, pipeline health — or 'everything')

Answer all at once or just say 'everything' for all.
```

### Step 3: Build the Observer dynamically

Based on the user's answers, generate the Observer's scan strategy for each MCP:

- Map user priorities to specific MCP tool calls
- For each MCP, define 2-3 surgical queries that target what the user cares about
- If user said "everything", design broad-but-limited discovery queries using the available tools
- Store the scan strategy in `System/mesh.md` so future Observer runs know what to query

### Step 4: Write vault files

Write using `references/` templates:
- `System/mesh.md` — inject discovered MCPs, user priorities, scan strategy
- `System/registry.md` — empty registry
- `System/Agents/observer.md` — generated dynamically from scan strategy (NOT from a static template)
- `System/Agents/architect.md` — from template
- `System/Agents/pruner.md` — from template

### Step 5: Run first observation

Execute the Observer immediately so the user sees value on first run.

### Step 6: Report

Files created, patterns found, what happens next.

## Observer

Scans selected MCPs using the strategy defined during bootstrap.

### Execution

1. **Read `System/registry.md`** — note existing agents to skip covered patterns
2. **Read scan strategy from `System/mesh.md`** — this tells you which MCPs and what queries
3. **Execute scan queries in parallel** — run the defined queries for each MCP (max 3 calls per MCP, use filters/limits/date ranges)
4. **Analyze results for patterns** — look for:
   - Recurring manual work (same person doing same thing repeatedly)
   - Bottlenecks (things stuck, waiting, overdue)
   - Data quality issues (missing fields, inconsistent formats)
   - Communication gaps (unanswered questions, status requests)
   - Stale items (old deals, old PRs, old issues)
5. **Manage rolling window** — if 3 observation files exist, compress oldest's recurring/chronic patterns to `System/digest.md`, then delete it
6. **Write observation** to `System/Observations/YYYY-MM-DD.md`:

```markdown
---
date: YYYY-MM-DD
sources: [MCP1, MCP2]
---
## Patterns
### <name>
source: <mcp> | freq: first/recurring/chronic | auto: high/med/low
<One sentence with specifics — names, numbers, dates>
```

7. Report top 3 patterns to user.

## Architect

Reads latest observation + digest. Spawns agents for strong patterns.

### Execution

1. **Read `System/registry.md`** — check agent count (max 10) and existing agents
2. **Read latest `System/Observations/` file** — just the newest, NOT all
3. **Read `System/digest.md`** if it exists — for recurring pattern history
4. **Score each unhandled pattern**:
   - Frequency × 3 (max 9) + Impact × 2 (max 6) + Feasibility × 2 (max 6) + Simplicity × 1 (max 3) = max 24
   - Threshold: ≥14 OR chronic
5. **Write agent spec** to `System/Agents/<name>.md` (≤30 lines):

```markdown
---
type: agent-spec
status: candidate
trigger: weekly
mcps: [<which MCPs>]
created: YYYY-MM-DD
score: 18
---
# <Name>
## Purpose
<One sentence>
## Steps
1. <Specific MCP tool call with parameters>
2. <Process results>
3. <Write to System/Logs/<name>.md>
## Kill when
<One sentence>
```

6. **Add row to registry**
7. Report: what was spawned and why.

## Execute

1. If agent named → read its spec from `System/Agents/<name>.md`
2. If "execute all" → read registry, run each active/candidate agent
3. Follow the agent's Steps — each step maps to specific MCP tool calls
4. **Overwrite** `System/Logs/<name>.md`:

```markdown
---
run_count: <N>
last_5: [date1, date2, ...]
status: success/error
---
<Output — under 100 lines>
```

5. Update registry: last_run, increment use_count
6. First successful candidate run → promote to active

## Pruner

Weekly. Never in full cycle.

1. Read `System/registry.md` + each agent's log from `System/Logs/`
2. Score: 3+ runs used → promote | 3+ ignored → kill | <3 → keep | 2 errors → kill | overlap → merge
3. Act: update registry, edit/delete specs
4. Overwrite `System/Logs/pruner.md` with one-page report
5. If <3 agents: lower architect threshold. If >8: raise it.

## Status

1. Read `System/registry.md` only
2. Show:

```
Mesh: 3 core | X active | Y candidates | Z essential
Last scan: YYYY-MM-DD | Last prune: YYYY-MM-DD
Agents: [name1 (active, 5 runs), name2 (candidate, 1 run)]
Health: [healthy / needs scan / needs prune]
```

## Principles

1. **MCP-agnostic** — works with any MCP combination, discovers at runtime
2. **Token-efficient** — read minimum files, write compact output, surgical queries
3. **Human-readable** — every agent is an editable markdown file
4. **Self-regulating** — Pruner kills waste, max 10 agents
5. **Rolling window** — 3 observations max, digest catches history
6. **Logs overwrite** — one file per agent, not an archive
7. **Dynamic Observer** — scan strategy built from user input, not hardcoded
