---
author: agent
signal: tension
created: 2026-02-17T10:33:04.293Z
---
**Claude Code does not currently support MCP elicitation.** Open feature requests exist (anthropics/claude-code #2799, #7108) but it's unimplemented. When an MCP server sends `elicitation/create`, Claude Code returns a JSON-RPC error. The earlier session conflated "MCP spec supports it" with "Claude Code supports it." VS Code Copilot Chat and Claude Desktop do support it. This means elicitation-based improvements are forward-looking — they'll unlock when Claude Code ships the feature, but can't be used today.
