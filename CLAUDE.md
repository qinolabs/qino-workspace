# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Workspace Overview

This is a multi-repo workspace. Navigate to the appropriate repo based on the task:

| Repo | Purpose | When to use |
|------|---------|-------------|
| `qinolabs-repo/` | Main monorepo with all apps | Building features, fixing bugs, app development |
| `qino-claude/` | Claude Code tools and plugins | Creating/updating Claude commands, agents, plugins |
| `qino-concepts/` | **Main concepts repository** | Concept exploration, app visions, ecosystem design |
| `qino-research/` | Research and documentation | Research tasks, technical writing |
| `qino-lingo/` | Conversation archives | Reference only |
| `qinolabs-legal/` | Legal documents | Legal/compliance work |
| `external-apps/` | External client apps | Standalone projects outside qinolabs ecosystem |
| `external-concepts/` | External project concepts | Design docs for external client work |
| `concepts-repo/` | **Deprecated** — being phased out | Legacy concepts, migrate to qino-concepts |
| `gamejoin-app/` | Standalone gamejoin prototype | Reference/comparison only |

**gamejoin-app disambiguation**: "gamejoin-app" refers to the app inside `qinolabs-repo/` unless explicitly stated otherwise (e.g., "compare with the gamejoin prototype" refers to the standalone `gamejoin-app/` at workspace root).

## Quick Navigation

**App development** (qino-world, qino-journey, etc.):
```
cd qinolabs-repo && cat CLAUDE.md
```

**Concept exploration** (app visions, ecosystem design):
```
cd qino-concepts && cat graph.json
```

**Technical architecture** (cross-app patterns):
```
cd qinolabs-repo/implementations/docs && cat README.md
```

**Claude tooling** (commands, agents, plugins):
```
cd qino-claude && cat CLAUDE.md
```

## Key Apps in qinolabs-repo

| App | Frontend Port | Backend Port | Purpose |
|-----|---------------|--------------|---------|
| qino-world | 3007 | 4007 | World exploration interface |
| qino-arc | 3010 | 4012 | Cross-modality manifestation browser |
| qino-journey | - | 5003 | Ecosystem-level encounter tracking |
| qino-frame | 3003 | 4003 | Design tool |
| qino-drops | 3001 | 4001 | Procedural idle canvas — figures emit drops onto a multi-track visualization |
| qino-chronicles | 3006 | 4006 | Story/chronicle system |

## App Concepts

### qino-drops: Procedural Idle Canvas

**Genre analogy**: Think idle games (Cookie Clicker, Mountain) meets generative art.

qino-drops is a **real-time simulation** where the user's personal figures (abstract concepts like "Outer Wilds", "morning coffee ritual", "François Jullien") emit visual artifacts called "drops" onto a multi-column canvas.

**Core mechanics**:
- **Figures** have simulation parameters: `verticalBias`, `horizontalBias`, `colorSeed`
- **Drops** emerge algorithmically and animate through phases: ghost → outline → forming → mature → fading
- **Cadence modes** control temporal pacing: still, ambient (15min), contemplative (5min), active (1min), intensive (20sec)
- **Sessions** contain the figures you're "watching" — you select figures, start a session, and observe emergence

**What it is NOT**: A contact manager, CRM, or address book. The word "relational" refers to how ideas relate to each other, not interpersonal relationships.

**Key insight**: It's a thinking tool — a way to sit with concepts and watch patterns emerge without actively manipulating them.

## Common Patterns

- **Frontend apps**: `qinolabs-repo/apps/{app-name}/`
- **Backend workers**: `qinolabs-repo/apps/{app-name}-backend/`
- **UI Core**: `qinolabs-repo/packages/ui-core/` — Basic universal components
- **Shared UI** (deprecated): `qinolabs-repo/packages/ui/` — Being phased out in favor of ui-core
- **Shared contracts**: `qinolabs-repo/packages/contracts/`

**Note**: The shared UI library (`packages/ui/`) is being phased out. Use `packages/ui-core/` for basic universal components. When working on UI, prefer ui-core for new components and migrate existing ui imports when touching that code.

## Navigators

Navigators are graph nodes (`type: "navigator"`) in the workspace root graph. They appear in qino-lab at `http://localhost:4020`.

Living orientation documents for cross-cutting possibility spaces being actively explored or built. Each navigator maps terrain with meaning, carries a reading order, tracks open questions, and accumulates a session log across sessions.

**When to use**: "use the active navigator", "navigate [territory]", "create a navigator"

**Not plans** (they don't prescribe tasks). **Not arcs** (arcs trace inquiry; navigators map terrain for building).

**Current active navigators**: Check the root graph for nodes with `type: "navigator"`.

## Implementation Notes & Iterations

**Location**: `qinolabs-repo/implementations/`

This is where all iteration plans and implementation documentation live — **regardless of which repo you're working in** (concepts-repo, qino-research, qino-claude, etc.).

**Structure per app** (follows qino-protocol):
```
implementations/{app}/
  node.json                      # Identity (title, type, status)
  story.md                       # Technical context, stack, boundaries, flags
  content/
    01-foundation.md             # First iteration
    02-feature-name.md           # Subsequent iterations
    ...
```

**Naming convention**: `{NN}-{descriptive-name}.md` (e.g., `01-metabolic-foundation.md`, `02-interaction-resonance.md`)

**When to read**:
- "What's the current iteration?" → `implementations/{app}/content/`
- "What are the technical boundaries?" → `implementations/{app}/story.md`
- "What's the essence/vision?" → `qino-concepts/nodes/{concept}/story.md`

**Active apps with implementations**: qino-drops, qino-world, qino-chronicles, qino-journey, qino-frame, qino-walk, qino-label, lens-lab, qino-infra

**Iteration index**: `qinolabs-repo/implementations/graph.json` lists all apps and their iterations in one place.

## Proposal Annotations

Feature ideas and observations that arise during work but shouldn't be acted on immediately are captured as **proposal annotations** on implementation nodes.

**Convention**:
- Signal: `proposal`
- Lifecycle: `open` (default at creation) -> `accepted` (will influence next iteration) -> `resolved` (built) or `dismissed` (not pursuing)
- Written to: `implementations/{app}/annotations/{NNN}-{slug}.md`
- Format: standard annotation frontmatter (`signal: proposal`, `created: YYYY-MM-DD`, optional `target:` for related iteration/concept)

**When to write**: During implementation work, when you notice a feature idea, cross-app opportunity, or architectural improvement that belongs in a future iteration rather than the current one.

**When to read**: Before planning a new iteration. Open proposals on the app's node carry accumulated insight from prior sessions.

**How to write**: Use the MCP `write_annotation` tool (signal: `proposal`, nodeId: the app id, graphPath: `qinolabs-repo`) or write the annotation file directly.

**How to resolve**: Use the MCP `resolve_annotation` tool or update the annotation's frontmatter (`status: resolved`, `resolvedAt: YYYY-MM-DD`).

## Technical Documentation

**Location**: `qinolabs-repo/implementations/docs/`

Settled technical architecture and cross-app patterns that serve as the **starting point for in-depth technical exploration**.

**For AI agents**: Read these first when exploring system design, integration patterns, or architectural decisions.

**For humans**: Deep technical reference for understanding how components communicate and why things work the way they do.

**Current documents**:
- `seeding-architecture.md` - Multi-level seeding (World, Arc, Journey)
- `journey-modality-integration.md` - Encounter recording and enrichment (planned)
- `rpc-service-bindings.md` - Worker-to-Worker communication patterns (planned)

**When to read**:
- "How does seeding work across the ecosystem?" → `implementations/docs/seeding-architecture.md`
- "How do modalities integrate with Journey?" → `implementations/docs/journey-modality-integration.md`
- "What are the RPC contracts between services?" → `implementations/docs/rpc-service-bindings.md`

**Relationship to other docs**:
- `implementations/{app}/` - App-specific implementation notes
- `implementations/docs/` - Cross-app architecture and patterns
- `qino-concepts/` - Concept exploration and vision (qino-protocol structure)
- `qino-research/` - Research and investigations

## Development Commands

From `qinolabs-repo/`:
- `pnpm dev:ecosystem` - Start all apps
- `pnpm -F @qinolabs/{package} dev` - Start specific package
- `pnpm -F @qinolabs/{package} check` - Typecheck + lint (token-efficient)

## Domain Language

**CRITICAL**: Use qino ecosystem terminology consistently.

**Source of truth**: `qino-concepts/ecosystem/qino-domain-language/story.md`

This document contains the full vocabulary: Figure, Manifestation, Substrate, Encounter, Lens, Scope, Contour, Current, Voicing, Facet, Crossing, Awakening, Lineage, Modality, Journey — and the narrative that shows how they relate.

**Key insight**: Manifestations are local presences; figures are patterns recognized across them.

## Design Principles

**Source of truth**: `qino-concepts/ecosystem/ecosystem-design-principles/story.md`

Meta-principles for qino system architecture, discovered through building. Includes "Trust the Ecosystem" and "Boundaries of Meaning."

**When to consult**: Before designing complex local solutions, or when integrating across modalities/layers.

## Qino Skill

Use the **qino skill** for work in this ecosystem:

**Dev work** — "what's next for [app]", "read the implementation notes", "what's the iteration status", "build [feature]"

**Navigation** — "use the active navigator", "navigate [territory]", "map this concept", "update the navigator"

**Concept work** — "explore [concept]", "where am I", "capture this thought", "test through ecology"

**Research** — "start research on [topic]", "investigate [question]"

**Explicit only** — Arc capture requires "/qino arc" or "capture an arc" (not implicit)

The qino skill routes to specialized agents (concept, dev, research) based on intent.
