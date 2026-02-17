# World Temporal Design -- Terrain

## I. Temporal Texture (The Facet)

The conceptual foundation: how worlds breathe through two layers of time, and why the crack between world time and user time is the richest design space.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Temporal texture facet** | `qino-concepts/concepts/qino-world/nodes/temporal-texture/content/temporal-texture.md` | The full design document: two layers (cyclical micro + scenario macro), universal time unit, cycle as encounter texture, the crack between world time and user time, temporal layout as design act, stories of what you missed, cycles as world design |
| **Concept.md -- The Interlude** | `qino-concepts/concepts/qino-world/content/concept.md` SS The Interlude | The user experience of scenario transition: immersive narration, relative change, decision points, memory as residue. The rhythm: Field -> Event -> Interlude -> New Field |
| **Concept.md -- Awakening as Seed** | `qino-concepts/concepts/qino-world/content/concept.md` SS Awakening as Seed | The evolved model: awakening is a seed sown with delay, introduction through existing fabric, unpredictability as design value, three awakening types (manifestation, area, world) |
| **Concept.md -- Mentions Awaken Through Curiosity** | `qino-concepts/concepts/qino-world/content/concept.md` SS Mentions Awaken Through Curiosity | Knowledge boundaries (distant/adjacent/intimate), relational inquiry, natural pull across relationships. The substrate accumulation model that temporal staging acts upon |

**Two layers of time, neither optional.** The cyclical layer (micro) provides perpetual rhythms -- shift changes, tidal turns, orbital phases. The scenario layer (macro) provides discrete chapters with conditions, accumulated history, shared understanding. Cycles run within scenarios. Scenario transitions can alter cycles. Neither waits for the other.

**The universal time unit.** All worlds share the same temporal heartbeat. What a tick *means* is world-specific: shift change on the station, tidal turn at the reef, orbital phase shift on the comet. The mapping from universal time to cyclical position is part of world design -- as intentional as naming areas or placing manifestations.

**Settled decisions:**
- World time ticks independently of user time (real-time, not user-driven)
- Temporal layout is a per-world design decision (alignment, phase shift, frequency)
- Cycles are as fundamental as areas and manifestations in world design

---

## II. Awakening Surfaces (What the User Experiences)

The design frontier: what does the user actually see and feel when something awakens? The *timing* is captured in the temporal texture facet (cycles provide the clock). The *surface* -- what the user experiences -- is the open territory.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Awakening system iteration** | `qinolabs-repo/implementations/qino-world/content/13-awakening-system.md` | What was originally built: substrate accumulation, awakening threshold, first speech generation, the frontend route. The ceremony model that has since evolved |
| **Substrate pipeline validation** | `qinolabs-repo/implementations/qino-world/content/25-substrate-pipeline-validation.md` | What's built and validated: Jaro-Winkler consolidation, LLM-classified depth, elaboration feedback, 4-deck awakening validation. Also documents the concept evolution from ceremony to seed model |
| **Awakening mechanics landscape** | `qinolabs-repo/implementations/qino-world/annotations/002-awakening-mechanics-landscape.md` | Open proposal: the missing mechanics between "substrate is warm" and "entity is introduced" -- staging logic, temporal mechanics, introduction through existing fabric |
| **Awakening service** | `qinolabs-repo/apps/qino-world-backend/src/services/awakening-service.ts` | Current implementation: first speech generation from accumulated perspectives. The code that needs to evolve to support temporal staging |
| **Substrate service** | `qinolabs-repo/apps/qino-world-backend/src/services/substrate-service.ts` | Substrate accumulation, Jaro-Winkler consolidation, awakening threshold check. The pipeline that determines *when* awakening becomes available -- but not *how* it surfaces |
| **Domain language -- Awakening** | `qino-concepts/concepts/qino-domain-language/content/domain-language.md` SS Awakening | Definition: "A latent presence becomes encounterable within its native modality... The world introduces the awakened entity through its existing fabric" |

**Three awakening surfaces, each with different weight:**

1. **Manifestation awakening** -- appears at the right cycle phase, in an appropriate area, introduced through existing fabric. A manifestation the user knows mentions seeing someone new near the docks during the supply run. The user may or may not be present when this happens -- if they missed it, they hear about it later. The most common awakening type.

2. **Area awakening** -- a location spoken of with enough depth across conversations eventually opens in the current world. Happens at scenario boundaries (the interlude reveals the new area as part of the world's shift). Less common, requires more diverse substrate sources.

3. **World awakening** -- rarest. Requires substrate from many sources across multiple scenarios. Appears as a passage in the interlude: "the traders speak of a place none of them have been." The accumulated references from manifestations, areas, and scenarios converge into a reachable destination.

**The pilgrimage pattern** ties all three together: the user hears stories about something, carries those stories, goes to a place at a certain time because of what they heard -- and the world recognizes their intentional presence. Stories become substrate the moment the user acts on them. The coincidence of presence is meaningful because the world has its own life.

---

## III. Scenario Progression (The Macro Heartbeat)

What drives scenario transitions? The interlude describes the *experience* (immersive narration, relative change, decision points). The temporal texture facet adds that scenarios have a temporal shape -- the world reaches its transition point through its own logic. But the *trigger mechanics* are thin in concept and absent in code.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Concept.md -- The Interlude** | `qino-concepts/concepts/qino-world/content/concept.md` SS The Interlude | The user experience of transition. Field -> Event -> Interlude -> New Field. Decision points fork. Memory becomes residue |
| **Temporal texture -- Two Layers** | `qino-concepts/concepts/qino-world/nodes/temporal-texture/content/temporal-texture.md` SS Two Layers of Time | How cyclical and scenario layers interact: cycles run within scenarios, scenario transitions alter cycles. "A siege changes the rhythm of supply runs" |
| **DeckScenario type** | `qinolabs-repo/packages/starter-decks/src/types.ts` | Current scenario data model: orderIndex, description, lore, currentTension, atmosphericTone, spatialCharacter, optional areas and layout. No temporal properties, no transition triggers |
| **Starter decks** | `qinolabs-repo/packages/starter-decks/src/decks/` | 13 decks with 1-3 scenarios each. Scenarios are ordered by index. Currently purely sequential with no transition logic |

**What exists:** Scenarios are ordered chapters. Each has description, tension, atmosphere, and spatial character. The system presents them sequentially (by `orderIndex`). The user experiences a scenario until... what triggers the next one?

**What the concept says:** The interlude is an experience -- immersive narration of what's unfolding. Time passes, things happen. The world has its own life. But the mechanism is unspecified: is it temporal accumulation (N ticks pass), substrate saturation (enough has happened), user engagement (sufficient conversations), world logic (internal conditions ripen), or a combination?

**What temporal texture adds:** Scenarios have a temporal shape. The world reaches its transition point through its own logic. The user may or may not be deeply engaged when the transition ripens -- the world continues either way. This suggests the trigger is primarily world-driven (temporal), not user-driven (engagement). But the user's engagement shapes *what* the transition resolves -- the accumulated substrate from conversations, awakening seeds, the stories that have been told all feed into the interlude's resolution.

**The interlude as resolution:** The interlude doesn't just narrate change -- it crystallizes accumulated state. Everything that happened during the scenario -- substrate, awakenings, decisions, stories told and missed -- becomes the material the interlude works with. An actively engaged user gets a richer interlude because more substrate has accumulated. An absent user gets a sparser but still valid transition -- the world moved on, here's what changed.

---

## IV. The Simulation Connection

The simulation framework can test temporal designs. Starter decks are the default substrate. User behavior profiles provide different temporal engagement patterns. The question: how do temporal mechanics show up in simulation?

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Simulation architecture** | `qinolabs-repo/implementations/docs/content/simulation-architecture.md` | Distributed orchestration: Lens Lab orchestrates, World executes. Turn-based simulation through the real dialogue pipeline |
| **Simulation profiles** | `qinolabs-repo/apps/lens-lab-backend/src/simulation-profiles.ts` | 4 profiles spanning the engagement spectrum: curious-explorer, lore-seeker, relationship-builder, speedrunner. Different profiles would interact differently with temporal mechanics |
| **Lens ecosystem navigator** | `nodes/lens-ecosystem/content/terrain.md` SS VII | The agentic framing: simulation as slot orchestration, modalities as outer lenses, starter decks as default substrate. The simulation pattern that would test temporal designs |

**How simulation could test temporal designs:**
- Run multi-scenario simulations where the world ticks between turns
- Different profiles visit at different "times" within cycles -- does cycle phase actually change dialogue quality?
- Test the pilgrimage pattern: does a lore-seeker who heard about the Rememberers during the quiet watch generate richer substrate when they actually visit during a quiet watch?
- Test scenario transition triggers: after N turns with sufficient substrate accumulation, does the interlude produce coherent resolution?
- Starter deck cycle definitions would become the test fixture -- each deck's temporal design is testable

---

## V. Implementation Gap Analysis

What exists, what's designed, and what's missing -- the delta that shapes the building frontier.

### Built and Working
- Substrate accumulation pipeline (mention -> natural detection -> substrate -> awakening check)
- First speech generation from accumulated perspectives
- Awakening threshold criteria (2+ unique perspectives, at least one intimate)
- Jaro-Winkler substrate consolidation
- LLM-classified knowledge depth
- 4-deck simulation validation (all produce awakenings)

### Designed in Concept, Not Yet Built
- Universal time unit (concept clear, no infrastructure)
- Cycle phases with manifestation resonance (concept clear, no data model)
- Cycle-aware dialogue (concept: manifestations shift with cycle phase; no prompt integration)
- Temporal staging of awakening (concept: wait for right cycle moment; code: instant when threshold met)
- Stories of what you missed (concept: manifestations narrate past events; no mechanism)
- Area and world awakening (concept: different substrate thresholds; code: only manifestation awakening)
- Scenario transition triggers (concept: world reaches its point; code: manual/sequential)
- Interlude as resolution experience (concept: immersive narration; no implementation)

### Not Yet Designed
- Concrete tick-to-real-time ratio (what range feels right?)
- How cycle phase flows into dialogue prompts (does the system prompt include current cycle state?)
- The awakening surface UX for each type (what does the user see when an area awakens?)
- Pilgrimage detection (how does the system know the user went somewhere because of a story?)
- Scenario transition trigger mechanics (temporal? substrate? engagement? combination?)
- How starter decks define cycles (what properties on DeckScenario or a new DeckCycle type?)
- Timezone handling for temporal alignment

---

## Reading Order

For entering this territory fresh:

1. **The temporal texture facet** -> `qino-concepts/concepts/qino-world/nodes/temporal-texture/content/temporal-texture.md` -- the foundation: two layers of time, universal time unit, cycle as encounter texture, the crack between world time and user time. Read this first because everything else builds on it.

2. **The interlude and awakening sections** -> `qino-concepts/concepts/qino-world/content/concept.md` SS The Interlude, SS Mentions Awaken Through Curiosity, SS Awakening as Seed -- the user experience that temporal mechanics serve. Read these to understand what the system should *feel* like.

3. **Domain language: Awakening** -> `qino-concepts/concepts/qino-domain-language/content/domain-language.md` SS Awakening -- the precise definition, including area/world types.

4. **Substrate pipeline validation** -> `qinolabs-repo/implementations/qino-world/content/25-substrate-pipeline-validation.md` -- what's built and proven. The concept evolution from ceremony to seed model. This is the current reality.

5. **Awakening system iteration** -> `qinolabs-repo/implementations/qino-world/content/13-awakening-system.md` -- the original implementation. Read to understand what exists in code, even though the concept has evolved past it.

6. **Awakening mechanics landscape** -> `qinolabs-repo/implementations/qino-world/annotations/002-awakening-mechanics-landscape.md` -- the open proposal that catalogs everything that's missing between "substrate is warm" and "entity is introduced."

7. **Awakening service + substrate service** -> `qinolabs-repo/apps/qino-world-backend/src/services/awakening-service.ts` and `substrate-service.ts` -- the code. First speech generation, substrate accumulation, threshold checking. Where temporal staging would be wired.

8. **Starter deck types** -> `qinolabs-repo/packages/starter-decks/src/types.ts` -- the data model that would need to accommodate cycle definitions. Currently no temporal properties.

9. **Lens ecosystem navigator, simulation section** -> `nodes/lens-ecosystem/content/terrain.md` SS VII -- the simulation framework for testing temporal designs. How starter decks serve as test fixtures and profiles provide engagement variation.

---

## Open Questions

### Universal Time Unit
- What is the tick-to-real-time ratio concretely? How many ticks per real hour?
- Does the ratio need to be per-world or is one ecosystem-wide ratio sufficient? (The facet says the unit is universal but the mapping is per-world -- what's the base frequency?)
- Should the tick be infrastructure (Journey records encounters in universal time) or application-level (World manages its own clock)?

### Cycle Integration into Dialogue
- How does cycle phase information flow into dialogue prompts? Does the system prompt include "you are currently in the quiet watch" for every message?
- Does the cycle phase affect which manifestations are available (some only during certain phases) or just their mood/attention?
- How do "stories of what you missed" get generated? Is it a special prompt mode when the user returns? Do manifestations accumulate events to tell?

### Awakening Surface Design
- What does the user see when a manifestation awakens? The current full-screen ceremony is too dramatic for the seed model. What's the right surface? A subtle shift in an area's description? A new figure appearing in the sidebar?
- What does area awakening look like in the UI? A new area appears on the relational field -- but when? During the interlude? Between sessions?
- What does world awakening look like? A passage mentioned in the interlude? A new entry in a world selector?
- How does the system distinguish the three types in its staging logic? Different substrate thresholds? Different temporal windows?

### Scenario Progression Mechanics
- What triggers scenario transitions? Is it temporal (N ticks), substrate-based (enough has accumulated), engagement-based (sufficient conversations), or world-logic-based (internal conditions)?
- How do cycles interact with scenario state? The facet says "a siege changes the rhythm of supply runs" -- is this authored per scenario or emergent?
- How does the interlude change based on engagement level? Deep presence vs. returning after absence?
- Should scenarios have explicit transition conditions in the starter deck data model? Or should transition be world-driven (the system decides when conditions are ripe)?

### The Pilgrimage Pattern
- How does the system recognize that a user went somewhere because of a story? Is explicit tracking needed (the system remembers which stories were told and where they pointed) or is it implicit (the substrate already captures this)?
- Is intentional presence different from coincidental presence in the system's eyes? The user who goes to the docks during the supply run because the Keeper told them about Lira -- vs. the user who happens to be at the docks when the supply run arrives. Should the system respond differently?
- How do stories become actionable? "You should have been here during the last supply run" -- does this give the user enough information to plan a visit? Or is the temporal opacity part of the design?

### Starter Deck Temporal Design
- How do starter decks define cycles? A new `DeckCycle` type? Properties on `DeckScenario`? A separate temporal configuration per world?
- What cycle properties are needed? Duration (in ticks), phases (named moments with qualities), manifestation resonance, area modulation, mention affinity?
- How does the simulation framework test temporal designs? Run simulations across different cycle phases and measure dialogue variation?

---

## Session Log

### 2026-02-16 -- Session 1: Navigator Created from Concept Crystallization

**What happened**: Created this navigator after concept work on the temporal-texture facet revealed that temporal texture, awakening surfaces, and scenario progression are one interconnected design space. The facet was written during this session; the navigator captures the full territory that emerged.

**Key moves**:
- Temporal texture facet created: two layers of time (cyclical micro + scenario macro), universal time unit, cycle as encounter texture, the crack between world time and user time
- Three awakening surface types distinguished: manifestation (mid-scenario, right cycle phase), area (scenario boundary), world (multi-scenario, interlude passage)
- The pilgrimage pattern articulated: stories the user carries become substrate the moment they act on them -- "the user hears a story, goes to the place at the right time, and the moment recognizes their intentional presence"
- Scenario progression identified as the thinnest design space: the interlude experience is designed but what *triggers* transitions is unspecified
- Implementation gap analysis: substrate pipeline is built and validated, but zero temporal infrastructure exists -- no time unit, no cycles, no temporal staging

**What was produced**:
- `qino-concepts/concepts/qino-world/nodes/temporal-texture/content/temporal-texture.md` -- the temporal texture facet
- Concept.md updated: "Awakening as Seed" section rewritten, thread reference to temporal-texture added
- This navigator node mapping the full territory

**What shifted**: Three previously separate design questions (how worlds have time, how awakening surfaces, what drives scenarios) revealed themselves as one system. Cycles provide micro-texture. Awakening uses cycles for staging. Scenario transitions are the macro heartbeat. The interlude resolves accumulated state. The pilgrimage pattern -- where stories become substrate for intentional presence -- is the experiential thread that ties them together.

**What remains alive**:
- The universal time unit needs a concrete specification (tick rate, infrastructure ownership)
- Cycle integration into dialogue prompts is the first buildable piece -- cycle phase as context for manifestation responses
- Scenario transition mechanics need design before building -- this is the thinnest space
- The pilgrimage pattern needs a detection mechanism -- how does the system know the user went somewhere because of a story?
- Starter deck temporal properties need a data model (DeckCycle type or equivalent)
- Simulation testing of temporal designs -- the framework exists but has no temporal dimension to exercise
