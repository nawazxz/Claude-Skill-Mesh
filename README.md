# Claude Skill Mesh

> A self-authoring AI agent mesh for Claude — detects your connected MCPs, spawns intelligent agents that observe patterns, and evolves through natural selection. Zero config. Works with ANY MCP stack.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Skill](https://img.shields.io/badge/Claude-Skill-blueviolet)](https://claude.ai)
[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-brightgreen)](https://modelcontextprotocol.io)

---

## What Is This?

**Claude Skill Mesh** is a single-file Claude skill (`SKILL.md`) that turns Claude into a living, self-organizing agent network. Point it at your tools and it:

1. Discovers all your connected MCP servers (Slack, GitHub, HubSpot, Notion, Linear, etc.)
2. Asks what you care about — then builds a custom observation strategy
3. Observes your systems for recurring patterns, bottlenecks, and manual work
4. Spawns specialized agents for every pattern worth automating
5. Prunes dead agents, promotes winners, keeps itself lean

All agents are plain Markdown files you can read, edit, or delete.

---

## Quick Start

1. Copy `SKILL.md` into your Claude project or paste it as a system prompt
2. Make sure you have at least one MCP connected (any will work)
3. Say: **`mesh`** or **`run mesh`**

That's it. Claude will do the rest.

---

## Commands

| Say this | What happens |
|---|---|
| `mesh` / `run mesh` | Bootstrap — discovers MCPs, sets up agents |
| `observe` / `scan` | Observer scans your systems for patterns |
| `architect` / `spawn` | Architect reviews patterns and spawns new agents |
| `run [agent-name]` | Execute a specific agent |
| `prune` | Pruner evaluates and culls underperforming agents |
| `mesh status` | Quick health check of the mesh |
| `mesh cycle` | Full cycle: observe -> architect -> execute all |

---

## How It Works

```
Bootstrap
  └── Discover MCPs -> Ask priorities -> Build scan strategy -> First observation

Observer (scheduled)
  └── Surgical MCP queries -> Pattern detection -> Rolling 3-file window

Architect (after observation)
  └── Score patterns (max 24) -> Spawn agents if score >= 14 -> Update registry

Execute
  └── Run agent steps -> Write log -> Promote candidate -> active

Pruner (weekly)
  └── Read registry + logs -> Promote | Kill | Merge -> Report
```

---

## Vault Structure

```
System/
  registry.md       -- Agent index (the only file read every run)
  mesh.md           -- Your MCP config + scan strategy
  digest.md         -- Compressed pattern history
  Agents/           -- Human-readable agent specs
  Observations/     -- Rolling window (max 3 files)
  Logs/             -- One file per agent, overwritten each run
```

---

## Token Efficiency

Mesh is engineered to stay fast and cheap:

- **Registry only** — one file read per run
- **Rolling observations** — max 3 files, oldest compressed to digest
- **Overwriting logs** — no log bloat, ever
- **Max 10 agents** — Pruner enforces the cap
- **Surgical queries** — max 3 MCP calls per tool, parallel where possible

---

## MCP Compatibility

Works out of the box with any MCP server:

- **Slack** — channel activity, unanswered threads, reporting
- **GitHub** — stale PRs, issue backlogs, review bottlenecks
- **HubSpot** — stale deals, missing data, pipeline health
- **Notion / Linear / Jira** — task patterns, blocked items
- **Any other MCP** — auto-discovered at runtime

---

## Agent Lifecycle

```
candidate -> active -> essential
               |
           graveyard
```

- **Candidate** — newly spawned, on probation
- **Active** — proven useful (3+ successful runs)
- **Essential** — used consistently, protected from pruning
- **Graveyard** — killed with documented reason

---

## References

The `references/` folder contains templates used during bootstrap:

| File | Purpose |
|---|---|
| `mesh-config.md` | Template for `System/mesh.md` |
| `observer.md` | Guide for generating dynamic Observer specs |
| `architect.md` | Template for `System/Agents/architect.md` |
| `pruner.md` | Template for `System/Agents/pruner.md` |
| `registry-template.md` | Template for `System/registry.md` |

---

## Contributing

PRs welcome! Ideas for improvement:

- New agent templates for popular MCP combos
- Smarter scoring heuristics for the Architect
- Digest compression strategies
- Multi-vault support

---

## License

MIT — use freely, attribution appreciated.

---

<p align="center">Built for the agentic era · Works with Claude · Powered by MCP</p>
