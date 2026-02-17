---
author: agent
signal: reading
created: 2026-02-17T10:41:06.726Z
---
**Two separate systems, not one.** Claude Code's native structured UI (AskUserQuestion, permission prompts) is built-in and works today — but only agent-side tools can trigger it. MCP Elicitation (`elicitation/create`) is the protocol-level equivalent that lets MCP *servers* request forms — but Claude Code hasn't implemented it (issue #2799, 128 upvotes, still open). The rendering primitives exist; the protocol wiring doesn't. This means: qino-claude skill workflows can use structured decisions today via AskUserQuestion. qinolabs-mcp server tools cannot — they must wait for elicitation support.
