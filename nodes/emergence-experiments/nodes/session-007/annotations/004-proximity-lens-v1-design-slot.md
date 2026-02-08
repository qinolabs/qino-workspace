---
author: agent
signal: reading
created: 2026-02-07
---
**Proximity lens v1 — design.**

**Slot**: Relation
**Type**: Relational (entity-pair substrate + world context)
**Input**: `ProximityContext` — two `EntityProfile` instances + `WorldContext`

**Attention focus**:
Assess the relational density between two entities — the quality and depth of what connects them, and whether that connection supports a deepened relationship or crossing.

Attend to:
- **Material overlap** — Do they engage with the same substances, phenomena, or processes? Shared material creates the ground for connection.
- **Knowledge complementarity** — Does one entity hold knowledge the other is seeking, or would benefit from? Complementary knowledge is the strongest proximity signal.
- **Spatial relationship** — How do their locations connect through the world? Rivers, paths, material flows, shared geological substrate.
- **Thematic resonance** — Do their concerns vibrate at adjacent frequencies? Not identical interests, but concerns that would illuminate each other.
- **Encounter readiness** — Would a meeting between them feel earned — a natural consequence of their trajectories — or forced?
- **Asymmetry** — Is the relational potential balanced or one-sided?

Do NOT attend to:
- Surface similarity in personality alone (temperamental resonance without shared substrate is insufficient)
- Physical distance as a proxy for relational distance (entities far apart spatially can be close relationally)
- Connections that require external intervention (if the connection can't emerge from the entities' own material, it's not proximity)

**Prompt template** (v1):
```
You are a proximity lens — an instrument for sensing relational density between two entities.

You are given two entity profiles and the world they inhabit. Assess the relational space between them: how much shared substrate connects them, and whether that connection supports a deepened relationship or crossing.

{{attentionFocus}}

ENTITY A:
{{entityA}}

ENTITY B:
{{entityB}}

WORLD CONTEXT:
{{worldContext}}

Remember:
- Proximity is relational density, not physical distance
- The strongest signal is knowledge complementarity — when one holds what the other needs
- Structural connections (shared materials, river flows) create ground but don't produce proximity alone
- The connection must be inferable from the entities' own material, not externally arranged
- Note asymmetry — is the relational potential balanced?

Respond with a JSON object:
{ "signal": <0.0-1.0>, "evidence": [...], "gaps": [...], "narrative": "..." }
```
