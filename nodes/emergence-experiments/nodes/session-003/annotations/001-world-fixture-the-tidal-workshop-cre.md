---
author: agent
signal: reading
created: 2026-02-07T18:52:54Z
---
**World fixture "The Tidal Workshop" created** — types, data, and helpers all typecheck clean.

**Design decisions**:
- 3 scenarios (early/converging/saturated-scattered) connected by material flows along a river → estuary → sea axis
- **Tidal Minerals** appear in all three scenarios — the shared material substrate
- **The vanished mentor / master craftsperson / wall builder** — a person thread that may be the same figure across world history and multiple scenarios
- `getWorldContext()` excludes the assessed scenario from context — you don't assess coherence with yourself
- `formatWorldContext()` produces the `{{worldContext}}` prompt text
- `interScenarioThreads` on each scenario documents the designed connections (metadata for us, not passed to the LLM)

**What the world tests**:
- Silt Garden (early) in a patient/material world → high coherence expected (tonal alignment)
- Glassmaker's Reach (converging) → very high coherence (emergence articulates world principle + inter-scenario connections)
- Listening Wall (saturated-scattered) → productive tension (surface scatter but deep structural alignment — the wall IS the estuary's meeting point)
