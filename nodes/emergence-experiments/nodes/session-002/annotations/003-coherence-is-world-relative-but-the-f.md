---
author: agent
signal: reading
created: 2026-02-07T22:37:48Z
status: resolved
resolvedAt: 2026-02-07
---
**Coherence is world-relative, but the fixtures are world-free.** The Full Market scored 0.32 in The Convergence Basin — but in a "Bazaar of Voices" world where scatter, cacophony, and parallel stories ARE the fabric, it would score 0.85+. The coherence signal is meaningless without specifying *which world*.

This creates a testing problem: we can't produce a universal coherence calibration table the way we did for convergence, saturation, and fertility. Those three lenses have fixture-intrinsic signals (convergence lives in the substrate, not the world). Coherence lives in the *relationship* between substrate and world.

**Implication for the pipeline**: Coherence assessment can never run in fixtures-only mode the way the other lenses do. It always needs a world context, even in testing. The fixture design needs to expand from 5 standalone scenarios to at least one test world containing multiple scenarios.

**Resolved**: Session 3 created The Tidal Workshop — a world-level fixture with 3 scenarios, `WorldContext` type, `getWorldContext()` helper, and `formatWorldContext()` prompt formatter. Session 4 confirmed coherence runs successfully against the world fixture. The standalone fixtures remain the calibration set for intrinsic lenses; the world fixture serves relational lenses.
