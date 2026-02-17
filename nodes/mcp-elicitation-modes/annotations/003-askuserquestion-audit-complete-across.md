---
author: agent
signal: reading
created: 2026-02-17T10:51:32.366Z
---
**AskUserQuestion audit complete across all plugins.** 11 decision points converted to structured UI. The pattern: structured choices add value at *routing* and *triage* points (disambiguation, multi-select from known options, recovery paths). They don't add value at *dialogue* points (orientation invitations, arc tracing, capture, compare sessions) where the conversational flow IS the feature.

**Implemented:**
- qino core (9): skill routing, navigator selection, composting triage, concept sync, drift recognition, iteration planning, draft detection, draft settling, ecology disambiguation
- qino-prose (2): chapter catastrophic failure recovery, arc selection for transmission

**Declined (intentionally conversational):**
- orientation, arc, capture, compare — voice design principles prevent menus
- chapter lens/scene checkpoints — user discusses/combines/adjusts options, not just selects
- qino-art routing — adaptive orchestrator handles ambiguity internally
- design-adventure — autonomous spawn, no user decisions
