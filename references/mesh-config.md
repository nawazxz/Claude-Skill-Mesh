# Mesh Config Template

Write to `System/mesh.md`, replacing placeholders.

```markdown
---
type: system
created: {{DATE}}
---
# Agent Mesh

## Connected MCPs
{{MCP_TABLE — dynamically generated from discovered MCPs}}

## Scan Strategy
{{For each selected MCP, list 2-3 queries based on user's answers during bootstrap. Example:}}

### <MCP Name>
- Priority: {{what user said matters}}
- Query 1: {{specific tool call + parameters}}
- Query 2: {{specific tool call + parameters}}
- Query 3: {{specific tool call + parameters}}

## Config
| Setting | Value |
|---------|-------|
| Max agents | 10 |
| Architect threshold | 14/24 |
| Observation window | 3 files |
| Pruner cadence | Weekly |
```
