# Difference Through Movement — Figures Across Environments

**Captured:** 2025-12-29T09:55:00Z

qino-world route architecture evolution: Implemented worlds/scenarios domain model, journey routes with split-view chat, and gallery entry flow. Key insight from Bateson: "recognizable figures in different environments creates difference which makes a difference" — sessions should have movement and transformation across scenarios.

---

## What Was Built

**Backend:**
- `worlds` and `scenarios` tables — a world contains multiple scenarios (locations/configurations)
- Sessions track `currentScenarioId` — where the journey currently stands
- `getJourneyState` returns embedded layout positions — no two-stage fetch
- `listWorlds`, `listSessions` for navigation

**Frontend Routes:**
```
/home                     - continue last + gallery preview
/worlds                   - world gallery
/worlds/$worldId          - world detail + scenarios
/sessions                 - user's sessions
/journey/$sessionId       - spatial canvas (split view ready)
/journey/$sessionId/chat/$figureId  - nested chat panel
```

**Key Patterns:**
- Split view: canvas shrinks to 60% when chat active
- Move mode via URL param: `?mode=move`
- Deep-linkable conversations
- Outlet pattern for nested routes

---

## The Bateson Thread

The route architecture isn't just navigation — it's designed for transformation. A session can traverse multiple scenarios, carrying the same figures through different environments.

"Recognizable figures in different environments creates difference which makes a difference."

The figures persist. The context changes. Meaning emerges from the delta.

This is why sessions need movement, not just state.

---

## Naming Tension: Two Journeys?

**qino-world journey:** Session traversing scenarios within a single world. Movement is *intra-app*, *intra-session*. The figures move through environments, but the container is one app, one session.

**Ecosystem journey:** Temporal context across apps. Movement is *inter-app*. This is what makes metaphor transport possible — drag the walk-discovered figure into the world canvas, carry a drops crystallization into a scribe chapter.

Are these the same architecture at different scales? Or fundamentally different?

The ecosystem journey needs:
- Cross-app entity identity (a figure is recognizable across contexts)
- Temporal ordering (what happened when, in what sequence)
- Metaphor movement protocol (how entities transfer between modalities)

The qino-world journey needs:
- Scenario state (which areas, which placements)
- Session continuity (same figures, evolving positions)
- Transition logic (how you move between scenarios)

**Hypothesis:** The qino-world journey is a *microcosm* of the ecosystem journey. Same pattern, smaller scope. If we get the architecture right at world-level, it might inform the ecosystem-level design.

Or: they rhyme but aren't identical. The ecosystem journey requires *translation* between modalities, while the world journey is translation-free (same canvas, different configuration).
