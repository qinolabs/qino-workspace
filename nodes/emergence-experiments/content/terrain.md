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

The Emergence Layer introduces 5 readiness lens slots — a new layer alongside Ecosystem, Context, and Weather in the lens-slot taxonomy. Each slot follows the same anatomy: input (what enters) → lens (how it's shaped) → output (what emerges) → effect (what it enables).

| Slot | Lens | Input | What It Reveals | Status |
|------|------|-------|-----------------|--------|
| **Readiness** | Convergence | Cross-source substrate | When multiple sources point at the same territory | Seeded (v1) |
| **Density** | Saturation | Any-scope accumulation | When enough has accumulated for next step | Seeded (v1) |
| **Integrity** | Coherence | Potential emergence + world history | Whether emergence fits established fabric | To author |
| **Closure** | Closure | Tension + conversation history | When tension has been sufficiently explored | To author |
| **Relation** | Proximity | Entity-pair shared substrate | When entities support deepened connection | To author |

### Seeded Lenses (From emergence-lab, migrated to lens-lab)

**Convergence** (v1):
- Attention focus: repeated themes across perspectives, shared entity mentions, aligned tension directions, complementary narrative threads
- Prompt template: Substitutes `{{substrate}}` and `{{scenarioContext}}`, asks for structured JSON assessment
- What it's sensitive to: Multiple independent sources pointing at the same territory

**Saturation** (v1):
- Attention focus: unique perspective count, knowledge depth distribution, mention breadth, awakened-to-unawakened ratio
- Prompt template: Same structure, different attention
- What it's sensitive to: Density and breadth of accumulated material

### Lenses To Author (Informed by running the first 2)

**Coherence** — Does potential emergence fit the established fabric? Needs to check against world history, existing manifestation relationships, scenario tone. The hardest lens — it requires contextual awareness beyond the immediate substrate.

**Closure** — Has tension been sufficiently explored? Not exhausted, but fulfilled. This lens needs to understand narrative arc — when has a question been lived with enough to release? The concept doc notes: the closure lens is the inflation guard. New scenarios can't emerge while current tension is still live.

**Proximity** — Do entities have enough shared substrate to support a deepened connection? This lens operates on entity-pair substrate rather than scenario-level. It reveals when relationships are ready to deepen — crossing territory.

### The Authoring Practice

Running convergence and saturation against the 5 fixture scenarios first reveals what "good assessment" looks like — signal values that distinguish stages, evidence that's specific rather than generic, gaps that point at real absences. The 3 unwritten lenses should be authored *after* building intuition from the first 2.

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

### Readiness Lens Definitions
- What are the concrete `attentionFocus` and `promptTemplate` for coherence, closure, proximity?
- Can readiness lenses be composed? ("Convergent AND coherent" as a combined assessment)
- Should lens sensitivity be tunable per-world or per-scenario?
- Are these the same construct as voicing lenses (same infrastructure, different slot) or do they need their own pipeline?
- What does a "good" assessment look like? How do we know when a lens is calibrated?

### Scenario Lifecycle
- When is a scenario "done"? Explicit user choice? Lens-detected closure? Both?
- What's the relationship between scenario completion and new scenario emergence?
- Can completed scenarios be revisited? Do they continue ticking?
- How much narrative state should the starter deck carry vs. emerge through play?

### Inflation and Pacing
- What's the right ratio of "scenarios explored" to "scenarios discovered"?
- Should discovery rate slow as the world grows?
- How do we prevent the world from becoming so large it loses coherence?
- Is there an equivalent of drops' cadence modes for world ticks?

### Tick Specification
- What's the input to a tick? Full world state? Only recent changes? A summary?
- How does the LLM maintain coherence across many ticks?
- Should ticks be deterministic given the same input, or intentionally stochastic?
- What happens to ticks when the user is mid-session?

---

## Reading Order

For entering this territory:

1. **The concept** → `qino-concepts/concepts/emergence-lab/content/concept.md` — the full vision (still the best orientation)
2. **The integration plan** → `qinolabs-repo/implementations/lens-lab/content/21-emergence-integration.md` — what was decided about merging
3. **The fixtures** → `qinolabs-repo/apps/lens-lab-backend/src/fixtures/emergence-scenarios.ts` — the 5 test scenarios (see the substrate)
4. **The seeded lenses** → `qinolabs-repo/apps/lens-lab-backend/src/services/readiness-lens-seeder.ts` — convergence + saturation definitions
5. **The assessment service** → `qinolabs-repo/apps/lens-lab-backend/src/services/assessment-service.ts` — how lenses are applied
6. **The old navigator** → `qinolabs-repo/navigators/emergence-lab.md` — sessions 1-3, terrain mapping, resolved questions

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
