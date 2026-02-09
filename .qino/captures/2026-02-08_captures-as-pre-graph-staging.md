# Captures as independent record at workspace root

**Captured:** 2026-02-08T22:00:00Z
**Origin:** concepts-repo migration — auditing capture lifecycle

Captures are a record of raw thinking moments — independent of concepts, research, or implementation. They don't need to become something else. They don't need status. They're already what they are.

The old protocol made captures heavyweight graph citizens (node.json + graph.json + edges). The fix: `.qino/captures/` at the multi-workspace root. Flat files. No lifecycle.

## Format

`.qino/captures/YYYY-MM-DD_essence.md` — title, timestamp, content. That's it.

## Composting

Pull-based, user-initiated. You're working on a concept, you remember a capture, you say "absorb this." The agent reads it, weaves what's relevant. The capture stays — it's still a record of that moment.

No scanning for "waiting" captures. No status tracking. No deletion pressure.

Like a physical notebook: you don't track which pages have been "absorbed." The notebook is its own artifact. Sometimes you flip back and something catches your eye.

## Protocol changes needed

- Capture workflow: write to `{root}/.qino/captures/`, acknowledge, done
- Home workflow: can mention recent captures exist, no obligation to process
- Explore workflow: user explicitly points to a capture when they want to absorb
- Workspace-init: create `.qino/captures/` at root
- qino-config: `workspaceRoot` or `capturesDir` path resolution

The `.qino/` directory is the protocol namespace at the workspace root — hidden, alongside `.claude/`. Protocol-level concerns (captures, potentially navigators and views) live here, separated from repos and transient files.
