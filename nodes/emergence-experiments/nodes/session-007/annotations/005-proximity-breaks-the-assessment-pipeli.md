---
author: agent
signal: tension
created: 2026-02-07
---
**Proximity breaks the assessment pipeline assumption.**

The current `assessment-service.ts` assumes all lenses receive `ScenarioSubstrate` as input:
- `assessScenario()` takes a scenario ID, gathers `ScenarioSubstrate`, applies a lens
- Template substitution uses `{{substrate}}` and `{{scenarioContext}}`
- Coherence added `{{worldContext}}` but still receives scenario substrate as the primary input

Proximity needs a fundamentally different input: two entity profiles assembled from potentially *different* scenarios, plus world context. There is no single "scenario" being assessed — the assessment target is the relational space between two entities.

This means the pipeline needs one of:
1. **A parallel assessment path** — `assessEntityPair()` alongside `assessScenario()`, with its own template substitution logic (`{{entityA}}`, `{{entityB}}`, `{{worldContext}}`)
2. **A generalized assessment path** — `assess(context: AssessmentContext)` where `AssessmentContext` is a discriminated union of scenario-level and entity-pair-level inputs
3. **Proximity as a separate service** — decoupled from the readiness assessment pipeline entirely

Option 2 is cleanest architecturally but requires refactoring the assessment service. Option 1 is more pragmatic and can be done additively. Option 3 would fragment the lens system.

The `EntityProfile` and `ProximityContext` types are committed. The `getEntityPairContext()`, `formatEntityProfile()`, and `formatWorldContext()` helpers are ready. What's missing is the pipeline path that uses them.
