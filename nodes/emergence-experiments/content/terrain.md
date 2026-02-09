# Emergence Experiments — Terrain

## I. The Domain Pattern: Accumulate → Assess → Emerge

The fundamental architecture is an **accumulation layer** (continuous, event-driven) paired with an **assessment layer** (periodic, evaluative). This pattern is fractal — it operates at every scale in the qino ecosystem:

| Scale | Accumulation | Assessment | Emergence |
|-------|-------------|------------|-----------|
| **Conversation** | Mentions surface in dialogue | Substrate checks depth | New manifestation (awakening) |
| **Scenario** | Events, dispositions shift, memories form | Tension evaluation via lenses | Scenario evolution, passage discovery |
| **World** | Cross-scenario traces, passage seeds | Coherence + readiness check | New scenario, new world |

The first row is built and working in qino-world (iterations 9-13: mention extraction → substrate tracking → relational inquiry → awakening). The mention-to-awakening pipeline IS this pattern at conversation scale. What emergence experiments develop is the same shape at scenario and world scale.

**The core insight**: Assessment is not a monolithic "is it ready?" check. It's **multiple lenses applied to the same accumulated substrate**, each sensitive to different qualities. A convergence lens doesn't add convergence — it makes convergence *visible*. A closure lens doesn't create resolution — it makes resolution *perceivable*.

**The tunability insight**: The system's behavior matures through lens calibration, not code changes. Each lens has `attentionFocus`, `promptTemplate`, and `version`. Tuning a readiness lens bumps the version, invalidating cached assessments. The system re-evaluates with the refined lens. Three stages: broad and generous (early), calibrated and discerning (tuned), world-specific (mature).

**References**:
- Concept: `qino-concepts/concepts/emergence-lab/content/concept.md` — sections 1-2
- Working implementation at conversation scale: `qinolabs-repo/implementations/qino-world/content/13-awakening-system.md`
- Domain language: `qino-concepts/ecosystem/qino-domain-language/story.md`

---

## II. The Instruments (What's Built)

The observation surface has been migrated from emergence-lab into lens-lab. Here's the current state of each component:

### Backend Assessment Pipeline

| Component | Location | Status |
|-----------|----------|--------|
| Assessment router | `lens-lab-backend/src/routers/assessment-router.ts` | Built — two data source modes (fixtures / real) |
| Assessment service | `lens-lab-backend/src/services/assessment-service.ts` | Built — fingerprint caching + OpenRouter LLM |
| Fingerprint service | `lens-lab-backend/src/services/fingerprint-service.ts` | Built — SHA-256 deterministic hashing |
| Readiness lens seeder | `lens-lab-backend/src/services/readiness-lens-seeder.ts` | Built — convergence + saturation lenses |
| Database schema | `lens-lab-backend/src/db/schema.ts` | Built — readiness_lenses + assessment_facets tables |
| WORLD service binding | `lens-lab-backend/wrangler.jsonc` | Configured — local, dev, prod |

**Data flow**: Select scenario → gather substrate → compute fingerprint → check cache → (miss) apply lens via LLM → store result → return assessment.

**Assessment output structure**: `{ signal: 0-1, evidence: string[], gaps: string[], narrative: string }`

**LLM integration**: OpenRouter → Claude Sonnet 4, temperature 0.3, max 500 tokens, structured JSON output.

### Frontend Substrate Workshop

| Component | Location | Purpose |
|-----------|----------|---------|
| Scenario selector | `lens-lab/src/features/substrate-workshop/scenario-selector.tsx` | Pick scenario, see context card with stage badge |
| Substrate inspector | `lens-lab/src/features/substrate-workshop/substrate-inspector.tsx` | View manifestations, mentions, relational inquiries |
| Signal meter | `lens-lab/src/features/substrate-workshop/signal-meter.tsx` | 0-1 readiness bar with color transition |
| Assessment display | `lens-lab/src/features/substrate-workshop/assessment-display.tsx` | Signal + evidence + gaps + narrative |

### Frontend Lens Workbench (Readiness Mode)

| Component | Location | Purpose |
|-----------|----------|---------|
| Readiness lens editor | `lens-lab/src/features/lens-workbench/readiness-lens-editor.tsx` | Author attentionFocus + promptTemplate |
| Readiness preview panel | `lens-lab/src/features/lens-workbench/readiness-preview-panel.tsx` | Test lens against fixture scenario |

### Fixture Scenarios (5 Designed Stages)

| Fixture | Stage | Profile |
|---------|-------|---------|
| The Gathering Threads | early-accumulation | 2 manifestations, 1 inquiry, 3 mentions |
| The Same Shore | converging | 3 manifestations, 4 inquiries |
| The Full Market | saturated-scattered | 5 manifestations, 7 mentions, 7 inquiries |
| The Threshold Garden | near-ready | 4 manifestations, 6 mentions, 6 inquiries |
| The Empty Room | cold | 1 manifestation, 1 mention, 0 inquiries |

Location: `lens-lab-backend/src/fixtures/emergence-scenarios.ts`

### World Fixture (The Tidal Workshop)

A world-level fixture for testing relational lenses (coherence, proximity). Three scenarios connected by material flows along a river-to-sea axis.

| Scenario | Stage | Manifestations | Inquiries | Inter-Scenario Threads |
|----------|-------|:-:|:-:|---|
| The Silt Garden | early-accumulation | 2 | 1 | Silt carries mineral waste from upstream, root patterns respond to tidal minerals |
| The Glassmaker's Reach | converging | 3 | 4 | Sand/mineral waste flows downstream, vanished mentor may be wall builder |
| The Listening Wall | saturated-scattered | 4 | 5 | Mineral veins match tidal minerals, wall builder used upriver traditions |

World theme: transformation through sustained contact (river meets sea). Connected by: tidal minerals (geological source), vanished mentor/wall builder (person thread), acoustic properties (sound carried by water).

**Design principle (Session 5)**: Characters have local knowledge only. Hale doesn't name all three locations — he describes a stone that hummed downstream. The coherence lens infers the connection; the characters don't announce it.

Location: `lens-lab-backend/src/fixtures/emergence-worlds.ts`

### New Types (Sessions 3, 7)

| Type | Location | Purpose |
|------|----------|---------|
| `WorldContext` | `lens-lab-backend/src/types.ts` | Third template variable for relational lenses |
| `EntityProfile` | `lens-lab-backend/src/types.ts` | Entity data assembled from scenario substrate for proximity |
| `ProximityContext` | `lens-lab-backend/src/types.ts` | Two entity profiles + world context — proximity lens input |
| `EmergenceWorld` | `lens-lab-backend/src/fixtures/emergence-worlds.ts` | World fixture type |

Helpers: `getWorldContext()`, `formatWorldContext()`, `getEntityPairContext()`, `formatEntityProfile()`, `buildEntityProfile()` — all in `emergence-worlds.ts`, typecheck clean.

### What Has NOT Been Verified

The migration is code-complete but the end-to-end loop has not been confirmed running:
- Seeding readiness lenses via the assessment router
- Loading fixture scenarios in the substrate workshop
- Applying a lens and seeing an assessment result
- Cache behavior (same substrate + lens version → cached)
- Workbench readiness mode authoring flow

### Iteration 21 Items Not Yet Built

Some aspects of the full iteration 21 plan remain:
- Scope selector in substrate workshop (Figure / Scenario toggle)
- Lens type selector in workbench (Voicing / Readiness toggle)
- Emergence Layer section in `/slots` route
- Substrate Flow section in `/slots`
- Emergence Layer tab in `/lenses` gallery
- Readiness experiment type in `/experiments`
- Scenario fixtures tab in `/test-data`

Full plan: `qinolabs-repo/implementations/lens-lab/content/21-emergence-integration.md`

---

## III. The Readiness Lenses (Emergence Layer)

The Emergence Layer introduces readiness lens slots — a new layer alongside Ecosystem, Context, and Weather in the lens-slot taxonomy. Each slot follows the same anatomy: input (what enters) → lens (how it's shaped) → output (what emerges) → effect (what it enables).

| Slot | Lens | Type | Input | What It Reveals | Status |
|------|------|------|-------|-----------------|--------|
| **Readiness** | Convergence | Intrinsic | `ScenarioSubstrate` | When multiple sources point at the same territory | Calibrated (v1), in seeder |
| **Density** | Saturation | Intrinsic | `ScenarioSubstrate` | When enough has accumulated for next step | Calibrated (v1), in seeder |
| **Potential** | Fertility | Intrinsic | `ScenarioSubstrate` | Whether latent connections exist that could become convergence | Calibrated (v1), drafted |
| **Closure** | Closure | Intrinsic | `ScenarioSubstrate` | When tension has been sufficiently explored — the inflation guard | Calibrated (v1), drafted |
| **Integrity** | Coherence | Relational | `ScenarioSubstrate` + `WorldContext` | Whether emergence fits established fabric | Calibrated (v2), drafted |
| **Relation** | Proximity | Relational | `ProximityContext` (entity-pair) | When entities support deepened connection — crossing territory | Calibrated (v1), drafted |

**Key classification**: 4 intrinsic lenses operate on scenario substrate alone. 2 relational lenses need context beyond the scenario. Proximity is architecturally unique — it operates on entity-pair substrate rather than scenario-level, requiring a new input shape (`EntityProfile` + `EntityProfile` + `WorldContext`).

### Calibration Table — Intrinsic Lenses (Standalone Fixtures)

Manual assessment of all fixtures through the 4 intrinsic lenses. These values serve as ground truth for evaluating the LLM pipeline.

| Fixture | Stage | Convergence | Saturation | Fertility | Closure |
|---------|-------|:-----------:|:----------:|:---------:|:-------:|
| The Empty Room | cold | 0.03 | 0.04 | 0.06 | 0.00 |
| The Gathering Threads | early-accumulation | 0.12 | 0.14 | 0.58 | 0.08 |
| The Same Shore | converging | 0.72 | 0.48 | 0.38 | 0.48 |
| The Full Market | saturated-scattered | 0.13 | 0.73 | 0.45 | 0.12 |
| The Threshold Garden | near-ready | 0.88 | 0.79 | 0.22 | 0.68 |

**Key finding**: Convergence and saturation are orthogonal — "The Same Shore" (0.72/0.48) and "The Full Market" (0.13/0.73) have inverted profiles. Direction without mass vs mass without direction. Near-ready requires both.

**Lifecycle interpretation**: Fertility behaves as potential energy. It peaks at early-accumulation (0.58) when threads are planted but unexplored, and decreases as inquiry converts latent connections into actual convergence/saturation. The four intrinsic lenses together reveal where a scenario is in its lifecycle, not just a static readiness score.

**Closure as inflation guard**: The Full Market (sat 0.73, closure 0.12) demonstrates the core pattern — abundant accumulation with almost nothing processed. Without closure, saturation is noise. Closure converts accumulation into readiness.

### Calibration Table — Coherence (Tidal Workshop)

Coherence is world-relative — the same scenario scores differently in different worlds. No universal calibration table is possible. Values below are for The Tidal Workshop.

| Scenario | Stage | Coherence (v2) |
|----------|-------|:--------------:|
| Silt Garden | early | 0.75 |
| Glassmaker's Reach | converging | 0.79 |
| Listening Wall | saturated | 0.58 |

Coherence v2 uses inference-over-matching attention: characters have local knowledge only, connections are inferred from circumstantial evidence. V1 scored Glassmaker's Reach at 0.92 (inflated by explicit cross-references); v2 corrects to 0.79.

**Latent coherence**: The Listening Wall (0.58) is structurally aligned with the world but experientially invisible — a distinct state from low coherence (contradiction). The wall doesn't contradict; it conceals.

### Calibration Table — Proximity (Tidal Workshop Entity Pairs)

Proximity operates on entity pairs, not scenarios. Values below are for pairs from The Tidal Workshop.

| Entity Pair | Scenarios | Proximity |
|-------------|-----------|:---------:|
| Seren ↔ Hale | Same (Glassmaker's Reach) | 0.85 |
| Hale ↔ Cael | Different (GR ↔ LW) | 0.78 |
| Moth ↔ Petra | Same (Listening Wall) | 0.62 |
| Ondra ↔ Seren | Different (SG ↔ GR) | 0.45 |
| Petra ↔ Seren | Different (LW ↔ GR) | 0.38 |
| Moth ↔ Ondra | Different (LW ↔ SG) | 0.15 |

**Knowledge complementarity** is the strongest driver — when one entity holds something the other needs. Temperamental resonance and co-presence contribute but are insufficient alone.

**Latent proximity**: Hale ↔ Cael (0.78) are structurally close but experientially invisible — the river carries Hale's frequencies to Cael's wall, but neither knows the other exists. Both relational lenses exhibit latent states.

### Calibrated Lenses

**Convergence** (v1) — calibrated:
- Attention focus: repeated themes across perspectives, shared entity mentions, aligned tension directions, complementary narrative threads
- Prompt template: Substitutes `{{substrate}}` and `{{scenarioContext}}`, asks for structured JSON assessment
- What it's sensitive to: Multiple independent sources pointing at the same territory
- Calibration: Correctly discriminates converging (0.72) from saturated-scattered (0.13). Strong at detecting multi-perspective alignment (Yael/Dhar/Tide Keeper in "The Same Shore"). Insensitive at low end (can't distinguish cold from early).

**Saturation** (v1) — calibrated:
- Attention focus: unique perspective count, knowledge depth distribution, mention breadth, awakened-to-unawakened ratio
- Prompt template: Same structure, different attention
- What it's sensitive to: Density and breadth of accumulated material
- Calibration: Correctly gives "The Full Market" (0.73) a high score despite no convergence. Counts voices, types, depths. Insensitive at low end (same limitation as convergence).

**Fertility** (v1) — drafted, not yet in seeder:
- Attention focus: complementary perspectives (origins/personalities suggesting different angles), thematic resonance across mentions (names/types hinting at latent connections), tension openness (invites multiple perspectives), unexpressed potential (voices waiting to speak), spatial affordances (convergence potential in setting)
- Prompt template: Explicitly instructs assessment of *potential energy*, not actual convergence or accumulation
- What it's sensitive to: Latent complementarity — connections that could form but haven't yet
- Calibration: Correctly gives "The Gathering Threads" (0.58) a high score where convergence and saturation both read low (0.12/0.14). Fills the low-end discrimination gap. Decreases as potential converts to actual (near-ready: 0.22).

**Closure** (v1) — calibrated:
- Attention focus: fulfillment quality (settling vs opening), breadth and depth of tension engagement, narrative arc sensitivity
- What it's sensitive to: Whether the scenario's `currentTension` has been explored with enough depth and variety to release
- Architectural finding: Closure is intrinsic, not relational. The substrate's `currentTension` + `relationalInquiries` provide enough for assessment without world context
- Calibration: The Full Market (sat 0.73, closure 0.12) is the key case — seven voices talking past each other, tension demonstrated but examined by no one. The Threshold Garden (0.68) shows partial closure: three voices have circled the gap from complementary angles but can't name what's missing

**Coherence** (v2) — calibrated:
- Attention focus: inference-over-matching. Structural resonance, material similarities, temporal coincidences, spatial logic — NOT explicit cross-references
- Input: `ScenarioSubstrate` + `WorldContext` (the `{{worldContext}}` template variable)
- What it's sensitive to: Whether emergence fits the world's established fabric — tonal alignment, thematic consistency, productive tension vs contradiction
- Calibration: Distinguishes pure alignment (Same Shore 0.82), productive tension (Threshold Garden 0.78), and contradiction (Full Market 0.32). World-relative — no universal calibration table
- **Storytelling integrity principle**: Characters have local knowledge only. The lens is the reader. Connections between scenarios must be inferred from circumstantial evidence, not announced by characters

**Proximity** (v1) — calibrated:
- Attention focus: material overlap, knowledge complementarity (strongest driver), spatial relationship, thematic resonance, encounter readiness, asymmetry
- Input: `ProximityContext` — two `EntityProfile` instances + `WorldContext`
- What it's sensitive to: Relational density between two entities — not physical distance but accumulated shared substrate quality
- Calibration: Cross-scenario pairs can score higher than same-scenario pairs (Hale↔Cael 0.78 > Moth↔Petra 0.62) because proximity is relational density, not co-presence
- **Pipeline tension**: Proximity breaks the `assessScenario()` assumption. Needs a parallel `assessEntityPair()` path or a generalized assessment context. Types and helpers committed; pipeline path not yet built

### The Authoring Practice

The authoring loop: **run → observe gap → author new lens → calibrate**. Each lens was authored in response to something the previous calibration revealed:

- Fertility: authored because convergence and saturation couldn't distinguish cold from early (low-end insensitivity)
- Coherence: authored because intrinsic lenses can't assess whether emergence fits the world (required new input shape: `WorldContext`)
- Closure: settled the architectural question (intrinsic, not relational — substrate carries enough) and revealed the inflation guard pattern
- Proximity: required the most architecturally distinct input shape (`ProximityContext`) and revealed latent states

All 6 lenses are now drafted and calibrated. None are in the seeder code except convergence and saturation. The LLM pipeline has never been run end-to-end.

**References**:
- Lens-slot architecture: `qino-concepts/ecosystem/tech-qino-lens/` (sub-graph with slots facet)
- Slot taxonomy: `qino-concepts/ecosystem/tech-qino-lens/nodes/slots/content/slots.md`
- Lens domain definition: `qino-concepts/ecosystem/qino-domain-language/story.md` (~line 133)

---

## IV. Ecosystem Connections

### Drops — Multiple Description

Drops implements the same *shape* in a simpler context. Seeing the same pattern through two descriptions reveals qualities invisible from either alone.

| Drops | Emergence |
|-------|-----------|
| Figure profiles (biases, colorSeed) | Starter deck (latent tensions, passage seeds) |
| Cadence mode (still → intensive) | Tick interval / rhythm |
| AI nudge generating DropSeeds | Assessment lenses evaluating readiness |
| Drops accumulating permanently | Substrate accumulating permanently |
| Session running without interaction | World evolving between sessions |

**What drops teaches**: Configuration over control. Cadence as mode of being, not speed setting. Accumulation creates meaning. Autonomous aliveness without urgency.

**Key reference**: `qino-concepts/concepts/qino-drops/` (concept + sub-graph)
**Technical reference**: `qinolabs-repo/apps/qino-drops-backend/src/coordination/drops-session.do.ts` (Durable Object tick loop)

### Arcs — Resonance Field

Arcs shape what assessment lenses perceive. An active arc doesn't control the world — it inflects how accumulated substrate is interpreted. The same substrate might be convergent through one arc's lens and diffuse through another's.

**Key distinction**: Arcs express in World as scenario-arcs, not ecosystem arcs. The mapping from ecosystem arc to scenario-level resonance is itself a lens operation.

**References**:
- Arc concept: `qino-concepts/concepts/qino-arc/` (sub-graph)
- Arc-world integration: `qinolabs-repo/implementations/qino-world/content/12-arc-aware-world.md`
- Attunement facet: `qino-concepts/concepts/qino-world/nodes/attunement/`

### World — Design Constraints

Emergence mechanics must respect these principles:

| Principle | Source | Constraint |
|-----------|--------|------------|
| **Pull mechanics** | world concept | Accumulation means something because you accumulated it. Emergence can't feel like unlocking achievements. |
| **Passages** | world concept | Discovered through relationship, not pre-placed. All four types: spatial, temporal, cross-modality, release. |
| **Attunement** | world facets | Arcs create resonance, not direction. No pressure, no meta-language from figures, no system UI breaking immersion. |
| **Crossing** | world facets | "Voice before face, relationship before either." New scenarios discovered through dialogue, not revealed by the system. |
| **No staged dialogue** | world story.md | Ticks produce traces, not conversations between figures. The world is encountered, not authored. |

**References**:
- World concept: `qino-concepts/concepts/qino-world/` (sub-graph with facets)
- Implementation boundaries: `qinolabs-repo/implementations/qino-world/story.md`

### The Mention → Awakening Pipeline (The Pattern Working)

The first expression of accumulate → assess → emerge, implemented and running:

| Step | What | Where |
|------|------|-------|
| Mention extraction | Substrate enters during dialogue | `qino-world-backend/src/services/mention-extraction-service.ts` |
| Substrate tracking | Perspective count, deepest knowledge | `qino-world-backend/src/db/schema.ts` → mentionSubstrate |
| Relational inquiry | Enrichment through different perspectives | same schema → relationalInquiries |
| Awakening | Substrate becomes manifestation | `qino-world-backend/src/services/awakening-service.ts` |
| Ceremony | Full-screen threshold moment | `qino-world/src/routes/_authed/session/$sessionId.awakening.$substrateId.tsx` |

---

## Open Questions

### Resolved
- ~~What does a "good" assessment look like?~~ → A good assessment discriminates between stages. Evidence references specific names and content.
- ~~Can readiness lenses be composed?~~ → Lenses compose as a *space* (convergence x saturation x fertility x closure). Each stage occupies a distinct region. Composition is spatial, not boolean.
- ~~What are the concrete attentionFocus and promptTemplate for coherence, closure, proximity?~~ → All three drafted with full lens designs (see Calibrated Lenses above).
- ~~Is the original 5-slot taxonomy complete?~~ → 6 slots: fertility fills the Potential slot.
- ~~Coherence input format?~~ → `{{worldContext}}` template variable, `WorldContext` type.
- ~~Does closure need sequential context?~~ → No. Closure is intrinsic — `currentTension` + `relationalInquiries` suffice.
- ~~Should the pipeline support different context shapes?~~ → Yes. Three shapes: scenario substrate (intrinsic), scenario + world (coherence), entity-pair + world (proximity).

### Pipeline Architecture
- Proximity needs a parallel assessment path (`assessEntityPair()`) or a generalized `AssessmentContext` discriminated union. Types committed, pipeline path not built.
- 4 drafted lenses (fertility, closure, coherence, proximity) are not yet in the seeder code.
- The LLM pipeline has never been run end-to-end against the calibration tables.
- Should lens sensitivity be tunable per-world or per-scenario?
- Are readiness lenses the same construct as voicing lenses (same infrastructure, different slot) or do they need their own pipeline?

### Lens Interiority (Exploration Thread)
- **Compositionality**: Lens readings compose into something no individual reading contains. The Full Market's profile (conv 0.13, sat 0.73, fert 0.45, closure 0.12) means "indigestion" — a metabolic state invisible from any single lens. This compositionality is prior to any particular grammar for reading it.
- **Multiple grammars**: The metabolic grammar (conversion ratio, digestion ratio) is the first grammar discovered but not the only one. Resonance, tension field, and compositional grammars read the same data differently. The choice of grammar is itself a lens operation.
- **Fractal claim**: The metabolic pattern holds at two scales (scenario-level and world-level). Each scale generates states the other can't name ("converting but not digesting" is visible only at world level). But the fractal hasn't been tested at entity-pair or meta levels.
- **Relational lenses absent**: Coherence and proximity aren't in any metabolic ratio yet. How do they participate in lens-to-lens composition?
- **Normative risk**: The metabolic framing risks treating states like "indigestion" as pathology. The Full Market might not be stuck — it might be accumulating toward a phase transition the current lenses can't predict.
- **NEW (annotation 007)**: Lenses consume each other — the network is alive, not static. Fertility generates what convergence finds. Convergence creates what saturation reads. The distinction between "lens" and "substrate" dissolves at certain scales. The correct framing is a digestive cycle, not a graph.

### Crossing Connection (Session 8 — partially resolved)
- ~~The proximity lens assesses readiness for crossing. How does proximity output inform crossing design?~~ → Session 8 traced Hale↔Cael (proximity 0.78) through full crossing mechanics. The proximity score is consumed by the receiving scenario's intrinsic profile, which determines the *kind* of crossing (convergence-accelerating vs fertility-catalyzing).
- ~~The NPC witness pattern is structurally the same as lensing. Is the witness a lens operation?~~ → **Yes.** The witness reads the arrival through the scenario's accumulated lens profile. "Voice before face" is literally the lens reading arriving before the entity does. Different witnesses apply different lens functions to the same arrival.
- ~~Can the full 6-lens profile function as a constraint surface for the crossing system?~~ → **Yes, via the consumption chain**: proximity → intrinsic profile → coherence → closure → witness selection → crossing character. Each layer metabolizes the previous. No individual reading determines the outcome.
- **NEW**: Closure gates absorption capacity. Below ~0.05, no witness exists. Above ~0.50, arrivals must conform. The 0.05–0.50 range is where crossings are most alive. Needs more calibration data beyond Tidal Workshop.
- **NEW**: The transformation service could receive lens profiles as crossing context — shaping (not constraining) how the AI interprets the figure against the scenario's state. Tension: over-determination kills surprise.
- **NEW**: Crossing is asymmetric — same pair, same proximity, fundamentally different crossings depending on direction (receiving scenario's profile determines meaning).
- **NEW**: How does a scenario's profile change AFTER a crossing? If Hale catalyzes convergence at the Listening Wall, the next assessment should reflect it. The crossing consumes the old profile and produces a new one.

### Scenario Lifecycle
- When is a scenario "done"? Explicit user choice? Lens-detected closure? Both?
- What's the relationship between scenario completion and new scenario emergence?
- The **blocked emergence profile** (high convergence + high saturation + low closure) is identified but no fixture represents it.
- Does the lifecycle interpretation (fertility → convergence/saturation conversion) inform when a scenario has "peaked"?

### Inflation and Pacing
- What's the right ratio of "scenarios explored" to "scenarios discovered"?
- Should discovery rate slow as the world grows?
- How do we prevent the world from becoming so large it loses coherence?

### qino-lab as Experiment Medium
- Annotation signals (reading, connection, tension, proposal) map structurally to readiness lens operations. Is this structural or coincidental?
- The reflexive experiment (applying lenses to the workspace graph itself) — deferred until instrument set is richer but now possible with 6 lenses drafted.

---

## Reading Order

For entering this territory:

1. **The concept** → `qino-concepts/concepts/emergence-lab/content/concept.md` — the full vision (still the best orientation)
2. **The integration plan** → `qinolabs-repo/implementations/lens-lab/content/21-emergence-integration.md` — what was decided about merging
3. **The standalone fixtures** → `lens-lab-backend/src/fixtures/emergence-scenarios.ts` — 5 test scenarios for intrinsic lenses
4. **The world fixture** → `lens-lab-backend/src/fixtures/emergence-worlds.ts` — The Tidal Workshop for relational lenses
5. **The types** → `lens-lab-backend/src/types.ts` — `WorldContext`, `EntityProfile`, `ProximityContext`
6. **The seeded lenses** → `lens-lab-backend/src/services/readiness-lens-seeder.ts` — convergence + saturation (only 2 of 6 in code)
7. **The assessment service** → `lens-lab-backend/src/services/assessment-service.ts` — how lenses are applied
8. **The crossing facet** → `concepts-repo/concepts/qino-world/facets/crossing.md` — how proximity becomes encounter
9. **The experiment log** → this sub-graph's nodes and annotations (sessions 1-8 + lens-interiority exploration)

---

## Session Log

### 2026-02-07 — Session 1: Navigator Created

**What happened**: Comprehensive preparation for picking up emergence experiments. Gathered all information on emergence-lab, lens-lab, and qino-lab MCP protocol. Discovered the lens-lab migration is largely code-complete (assessment pipeline, substrate workshop, fixtures, readiness lens editor all built). Created this navigator as the first node in the root workspace graph, superseding the old `emergence-lab.md` navigator.

**Key recognitions**:
- The infrastructure migration happened but was never verified end-to-end
- The qino-lab MCP protocol's role in emergence experiments is genuinely open — to be discovered through practice, not prescribed
- All 4 open question clusters from the old navigator remain alive
- 3 of 5 readiness lenses still need authoring (coherence, closure, proximity)

**What was produced**:
- This navigator node in the root workspace graph
- Edges to emergence-lab and qino-lab concept nodes
- Comprehensive terrain mapping with current-state file references

**What remains alive**:
- Verify the end-to-end assessment loop in lens-lab
- Run convergence and saturation against fixture scenarios to build intuition
- Author coherence, closure, proximity lenses informed by first experiments
- Discover what role the qino-lab protocol plays in this practice

### 2026-02-07 — Session 2: Manual Lens Calibration + Fertility Discovery

**What happened**: Ran convergence and saturation lenses manually against all 5 fixture scenarios, producing 10 structured assessments. Built a calibration table as ground truth. Discovered low-end insensitivity (cold and early-accumulation both in the low/low quadrant). Authored a **fertility** lens to fill the gap, ran it against all 5 fixtures, confirmed it discriminates early-accumulation (0.58) from cold (0.06).

**Key recognitions**:
- Convergence and saturation are orthogonal — they create a 2D readiness space where each stage occupies a distinct quadrant
- Fertility behaves as potential energy — peaks when threads are planted but unexplored, decreases as inquiry converts potential to actual convergence/saturation
- The three lenses together reveal lifecycle position, not just static readiness
- Annotation signals in qino-lab map structurally to readiness lens operations (connection = micro-convergence, tension = micro-gap-detection)
- The Full Market's hidden fertility (0.45) reveals the Night Market as a cross-domain convergence seed — a lens that detects what *to do next*, not just where things stand

**What was produced**:
- Session node in emergence-experiments sub-graph (session-001)
- 5 annotations: 2 readings (calibration table, fertility results), 1 connection (annotation signals ↔ readiness lenses), 1 tension (low-end insensitivity), 1 proposal (three next moves)
- Calibration table (ground truth for LLM pipeline evaluation)
- Fertility lens definition (v1 draft, not yet in seeder code)
- Updated terrain document (this update)

**What remains alive**:
- Author coherence lens — the first lens requiring context beyond scenario substrate
- Add fertility lens to the seeder code
- Run the LLM pipeline against calibration table to verify infrastructure
- Author closure and proximity lenses
- The reflexive experiment (applying lenses to the workspace graph itself) — deferred until instrument set is richer

### 2026-02-07 — Session 3: World Fixture Design

**What happened**: Created "The Tidal Workshop" — a world-level fixture with 3 scenarios connected by material flows along a river-to-sea axis. `WorldContext` type, `getWorldContext()` helper, and `formatWorldContext()` prompt formatter typecheck clean.

**What was produced**: `emergence-worlds.ts` with full world fixture, `WorldContext` type in `types.ts`, helper functions for world context extraction and formatting.

**Key recognition**: Relational lenses need world fabric, inter-scenario threads, and history. The Listening Wall is the most interesting test case — surface scatter concealing deep structural coherence.

### 2026-02-07 — Session 4: Coherence x Tidal Workshop

**What happened**: Ran coherence against all 3 world fixture scenarios. Coherence v1 discriminates well: tonal alignment (0.75), world-articulating emergence (0.92), latent coherence masked by scatter (0.58).

**Key recognitions**:
- The Listening Wall revealed *latent coherence* — structurally aligned but experientially invisible. Different from low coherence (contradiction).
- The Glassmaker's Reach 0.92 was likely inflated by Hale explicitly naming all three scenario locations — a fixture design issue, not a lens issue.

### 2026-02-07 — Session 5: Storytelling Integrity

**What happened**: Audited all 10 Tidal Workshop inquiries. Found 1 clear violation and 1 borderline case. Revised both. Established the Storytelling Integrity Principle. Drafted coherence v2 with inference-over-matching attention. Re-assessed Glassmaker's Reach: 0.79 (down from 0.92).

**Design principle**: Characters have local knowledge only. The lens is the reader. Connections are inferred from circumstantial evidence, not announced by characters.

### 2026-02-07 — Session 6: Closure Lens

**What happened**: Settled closure as intrinsic (not relational). Drafted lens with narrative sensitivity focus. Calibrated against all 5 standalone fixtures.

**Key recognitions**:
- The Full Market (sat 0.73, closure 0.12) is the inflation guard demonstration — "too full to think"
- Blocked emergence profile identified: high convergence + high saturation + low closure. Not yet represented in fixtures.

### 2026-02-07 — Session 7: Proximity Lens

**What happened**: Authored the final lens. Discovered a new input shape (`ProximityContext` with two `EntityProfile` instances). Calibrated against 6 entity pairs from the Tidal Workshop spanning 0.15–0.85.

**Key recognitions**:
- Knowledge complementarity is the strongest proximity driver
- Latent proximity (Hale↔Cael 0.78) parallels latent coherence — both relational lenses exhibit "ready but hidden" states
- Pipeline tension: proximity needs a different assessment path from scenario-level lenses

### 2026-02-07 — Lens Interiority Exploration (parallel thread)

**What happened**: Sparked by journal notes in sessions 5 and 6 about "the relationship of lenses themselves." Designed metabolic ratios (conversion, digestion) from existing calibration data. Ran fractal test — metabolic pattern holds at two scales (scenario-level and world-level). Discovered "converting but not digesting" as a world-level state invisible from individual scenarios.

**Key finding**: Compositionality is prior to any particular grammar. Lens readings compose into something no individual reading contains. The metabolic grammar is the first grammar discovered, but not the only one — resonance, tension field, and compositional grammars are alternatives. The choice of grammar is itself a lens operation.

### 2026-02-08 — Session 8: Lens Readings as Crossing Constraints

**What happened**: Traced the Hale ↔ Cael pair (proximity 0.78) through crossing mechanics from `crossing.md`. Discovered the full consumption chain: proximity → intrinsic profile → coherence → closure → witness selection → crossing character. Each layer metabolizes the previous.

**Key recognitions**:
- Crossing is asymmetric: Cael → Glassmaker's Reach = convergence acceleration; Hale → Listening Wall = fertility catalysis. Same pair, opposite meanings
- The NPC witness IS a lens function: "voice before face" is literally the lens reading arriving before the entity
- Closure gates absorption: below ~0.05 no witness exists, above ~0.50 arrivals must conform, 0.05–0.50 is the live zone
- Witness choice is a final lens selection: Hale witnessing = frequency recognition, Seren witnessing = cross-modal curiosity, Moth witnessing = territorial challenge
- The consumption chain makes compositionality concrete — what lens-interiority described abstractly, crossing reveals as a world event

**What was produced**:
- Session-008 node with 7 annotations (3 readings, 2 connections, 1 proposal, 1 product-bridge connection)
- Lens-interiority annotation 007 (lenses consume each other — alive network, not static graph)
- Lens-lab product mapping: iteration 21 refinements + iteration 22 scope

**Product bridge**: Session 8 informed lens-lab next steps:
- All 6 lenses drafted — seed into assessment service (iteration 21)
- Three input shapes confirmed — assessment service expansion (iteration 21)
- Profile view needed for compositionality — compound lens display (iteration 22)
- Entity-pair assessment path — `assessEntityPair()` (iteration 22)
- Crossing simulation workshop — consumption chain as interactive surface (iteration 22)

### 2026-02-08 — Lens Interiority Update (annotation 007)

**What happened**: Captured the user's intuition about lenses consuming each other. The "graph of lenses" framing implies static nodes/edges. What the fractal test and session 8 showed is a digestive cycle — each lens metabolizes the substrate that other lenses have shaped. The distinction between lens and substrate dissolves at certain scales.

**What remains alive across all threads**:
- ~~Proximity → Crossing pipeline~~ → **Resolved** by session 8 (consumption chain)
- ~~The witness pattern as a lens operation~~ → **Resolved** by session 8 (witness IS a lens function)
- ~~Lens profiles as crossing constraint surfaces~~ → **Resolved** by session 8 (the full chain is traced)
- Pipeline implementation (assessEntityPair path, seeder expansion, end-to-end verification)
- Multiple grammars for reading lens composition
- The reflexive experiment (lenses applied to the workspace graph)
- **NEW**: Post-crossing profile change — how do lens readings shift after a crossing event?
- **NEW**: Crossing simulation in lens-lab — interactive surface for the consumption chain (iteration 22)
- **NEW**: Lenses as alive network — the distinction between lens and substrate dissolves at scale
