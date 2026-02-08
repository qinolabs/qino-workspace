---
author: agent
signal: reading
created: 2026-02-07
---
**Proximity calibration table — 6 entity pairs from the Tidal Workshop.**

| Entity Pair | Scenarios | Signal | Key Factor |
|-------------|-----------|:------:|------------|
| Seren ↔ Hale | Same (Glassmaker's Reach) | 0.85 | Complementary sensory approaches to same material |
| Hale ↔ Cael | Different (GR ↔ LW) | 0.78 | Shared phenomenal domain, river upstream → downstream |
| Moth ↔ Petra | Same (Listening Wall) | 0.62 | Complementary knowledge types, frame collision risk |
| Ondra ↔ Seren | Different (SG ↔ GR) | 0.45 | Temperamental resonance, structural material connection |
| Petra ↔ Seren | Different (LW ↔ GR) | 0.38 | Shared minerals, possible person thread, backward-looking |
| Moth ↔ Ondra | Different (LW ↔ SG) | 0.15 | No shared material or thematic overlap |

The lens discriminates across the full range. The strongest driver of proximity is **knowledge complementarity** — when one entity holds something the other needs. Material overlap and co-presence contribute but are insufficient alone (Moth ↔ Petra share the same wall but score only 0.62 due to frame collision).

**Architectural finding**: Proximity required a new input shape — `EntityPairContext` with two `EntityProfile` instances plus `WorldContext`. This is the only lens that doesn't operate on scenario-level substrate. Types and helpers typecheck clean.
