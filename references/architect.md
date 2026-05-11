# Architect Agent Spec

Write to `System/Agents/architect.md`.

```markdown
---
type: agent-spec
role: architect
status: active
mcps: [vault]
created: {{DATE}}
---
# Architect
Reads latest observation + digest. Spawns agents for patterns scoring >=14/24 or chronic.

## Scoring
Frequency x3 + Impact x2 + Feasibility x2 + Simplicity x1 = max 24

## Rules
- Max 10 spawned agents
- Specs <=30 lines
- New agents start as candidate
- Each spec must include kill criteria
- Never create agents for one-time events
```
