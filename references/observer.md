# Observer Agent Spec

Write to `System/Agents/observer.md`. Generate dynamically — do NOT use a static template.

The Observer spec must be built from the scan strategy in `System/mesh.md`.

```markdown
---
type: agent-spec
role: observer
status: active
mcps: {{list of selected MCPs}}
created: {{DATE}}
---
# Observer
Scans connected MCPs for patterns. Never acts — only writes observations.

## Scan Strategy
{{Generated from bootstrap — list each MCP with its 2-3 specific queries}}

### <MCP 1>
- Query 1: <exact tool call + filters + limits>
- Query 2: <exact tool call + filters + limits>

### <MCP 2>
- Query 1: <exact tool call + filters + limits>
- Query 2: <exact tool call + filters + limits>

## Output
`System/Observations/YYYY-MM-DD.md` — compact 3-line patterns.
Rolling window: max 3 files. Oldest compressed to `System/digest.md`.
```

The key: every query is specific to the user's MCPs and priorities. Nothing is hardcoded.
