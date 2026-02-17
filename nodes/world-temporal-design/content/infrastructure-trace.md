# Infrastructure Trace: Where Cycles Touch the System

> The full path from starter deck definition through Qino Arc to setting up a scenario in Qino World, going through the crossing threshold, and exploring the world — with every surface where temporal texture makes contact identified and characterized.

**Session**: 2026-02-16
**Method**: Read every file along the path, tracing data flow and prompt construction. Each contact point identified by reading the actual code, not by inference.

---

## The Path

```
StarterDeck (authored)
  → Arc Deck Browser (presented)
    → Weave seed-router (dispatched)
      → World seedFromDeck (seeded into DB)
        → Session Creation (user enters world)
          → Crossing Ceremony (threshold)
            → Dialogue (exploration)
              → Mention Extraction → Substrate → Awakening
```

Each step below names the files, describes the current state, identifies where temporal texture makes contact, and characterizes the complexity of the change.

---

## 1. Starter Deck Definition

**Files:**
- `packages/starter-decks/src/types.ts` — all deck types
- `packages/starter-decks/src/decks/{deckId}/index.ts` — each deck's barrel export

**Current state:**

A `StarterDeck` is a static data structure:

```typescript
interface StarterDeck {
  id: string;
  name: string;
  description: string;
  audienceEmphasis: string;
  artDirection: string;          // Visual identity for image generation
  worlds: DeckWorld[];
  scenarios: DeckScenario[];
  areas: DeckArea[];
  manifestations: DeckManifestation[];
  walkGuidance: DeckWalkGuidance[];
  crossings: DeckCrossing[];
  layouts?: WorldLayout[];
}
```

`DeckWorld` has: `templateId`, `name`, `description`, `atmosphericQualities`, `deckId`.
`DeckScenario` has: `worldTemplateId`, `orderIndex`, `name`, `description`, `lore`, `currentTension`, `atmosphericTone`, `spatialCharacter`, optional `areas[]`, optional `layout`.
`DeckManifestation` has: `name`, `origin`, `personality`, `visualPresence`, `deckId`, `scope`, `presentIn[]`, optional `locations`, optional `crossWorldResonance`.

**No temporal data exists anywhere in the deck types.** Scenarios have `orderIndex` (sequence) but no transition logic, no cycle definitions, no temporal properties.

### CYCLE CONTACT: Deck Types

This is where cycle definitions would be **authored**. The deck designer needs to express:

- **What cycles this world has** — shift changes, tidal rhythms, orbital phases, market days
- **Phase definitions** — how many phases per cycle, their names and qualities
- **Manifestation resonance** — which manifestations are most alive during which phases
- **Area modulation** — how areas shift their character across phases
- **Tick-to-cycle ratio** — how world time maps to real time (fast: 4 phases/day, slow: weekly)
- **Mention affinity** — which phases are natural introduction windows for awakening

**Design space:**

| Approach | Shape | Trade-off |
|----------|-------|-----------|
| `DeckCycle[]` on `StarterDeck` | Top-level array, referenced by worldTemplateId | Clean separation; cycles are first-class alongside worlds/scenarios |
| `cycles` property on `DeckWorld` | Nested inside world | Tight coupling, clear ownership — cycles are a world property |
| `DeckTemporalProfile` per world | Separate type grouping all temporal properties | Rich structure but potentially over-engineered for v1 |

**Key question:** Manifestation resonance references per-manifestation data (who comes alive when), but cycles are per-world. The join is: `DeckManifestation.presentIn` (which worlds) × `DeckCycle.phases` (which moments). Where does the resonance mapping live — on the manifestation, on the cycle phase, or as a separate join table?

**Complexity: Medium** — new type design, but follows established patterns.

---

## 2. Arc Deck Browser

**Files:**
- `apps/qino-arc/src/routes/_authed/decks.index.tsx` — bundle/deck browser
- `apps/qino-arc/src/features/deck-browser/deck-card.tsx` — card display
- `apps/qino-arc/src/features/deck-browser/deck-data-adapter.ts` — data transformation

**Current state:**

User browses decks in a card slider. Each deck card shows:
- Badge: "Starter Deck 001"
- Visual: mosaic of 5 preview images
- Stats: worlds count, figures count, scenarios count
- Tap action triggers `seedMutation.mutate({ deckId, modality: "world" })`

### CYCLE CONTACT: Deck Presentation

The deck browser currently shows static counts. A temporally-rich deck has another quality to preview: *what kind of time does this world have?* The pace (fast cycles vs. slow), the character of the rhythm (tidal vs. industrial vs. orbital).

This is atmospheric information that helps users choose a deck. Not urgent for the data model — this is a UI concern downstream of having cycle data available.

**Complexity: Low** — display-only, depends on deck types having cycle data.

---

## 3. Seeding: Arc → Weave → World Backend

**Files:**
- `qino-infra/weave/src/routers/seed-router.ts:37-104` — Weave dispatch
- `apps/qino-world-backend/src/index.ts:172-536` — `seedFromDeck()` implementation

**Current state:**

Call flow: Arc frontend → Weave `seedFromDeck` → `ctx.env.WORLD.seedFromDeck(deckId, entityId, userId)`

Weave is pure dispatch. World backend's `seedFromDeck()` creates entities in this order:

1. **Worlds** — by `templateId`, stores: `name`, `description`, `templateId`, `imagePath`
2. **Scenarios** — by `worldId + orderIndex`, stores: `name`, `description`, `orderIndex`, `lore`, `currentTension`, `atmosphericTone`, `spatialCharacter`
3. **Areas** — scenario-scoped first, fallback to deck-level. Stores: `templateId`, `scenarioId`, `name`, `description`, `slotCount`, `imagePath`
4. **Layouts** — scenario-scoped first, fallback to deck-level. First scenario's layout marked `isActive`
5. **Manifestations** — filtered by `presentIn`. Stores: `name`, `origin`, `personality`, `visualPresence`, `imagePath`, `deckId`, `scope`, `presentInWorlds`. Records encounter in Journey for each.

**Idempotency guarantees:**
- Worlds: by `templateId`
- Scenarios: by `worldId + orderIndex`
- Areas: by `templateId + name`
- Manifestations: by `name + deckId`

### CYCLE CONTACT: World Seeding

Cycle definitions need to be seeded alongside worlds. When a world is created in the `worlds` table, its cycle configuration should be stored.

**Architectural insight: Cycle phase is computable, not stored.** Since "world time ticks independently in real time" is settled, the current phase is always derivable from:

```
currentPhase = computePhase(cycleDefinition, epochAnchor, Date.now())
```

The database stores the **definition** (what cycles exist, their durations, phase names) + an **epoch anchor** (when cycle counting began). The **state** (current position) is computed at read time. No cron jobs, no state synchronization, no ticking mechanism.

**Storage options:**

| Approach | Table | Shape |
|----------|-------|-------|
| New `worldCycles` table | Separate from worlds | `worldId`, `name`, `durationTicks`, `phases[]` (JSON), `epochAnchor`, `resonanceMap` |
| JSON column on `worlds` | Inline on world row | `cycleConfig: { cycles: [...], epochAnchor: number }` — simpler but less queryable |
| New `cycles` + `cyclePhases` tables | Fully normalized | Maximum queryability, but heavyweight for what's essentially authored content |

The JSON column approach matches the existing pattern (`layouts.areas` is stored as JSON `AreaPositionRecord`). Cycle definitions are authored content that's read whole, rarely queried by individual field. The `worldCycles` table approach gives better separation if cycles need independent CRUD.

### CYCLE CONTACT: Scenario Seeding

Scenarios currently get seeded with static context fields. With temporal texture, scenarios might also carry:

- **Active cycle modifiers** — a blockade scenario suppresses supply run cycles
- **Cycle configuration overrides** — different phase names or durations under different scenario conditions
- **Transition readiness properties** — what substrate signals indicate this scenario is ripe for transition (not implemented as data yet, but the scenario row is where it would anchor)

**Complexity: Medium** — schema migration for worlds table (cycle definition) + potential scenario extensions.

---

## 4. Session Creation

**Files:**
- `apps/qino-world-backend/src/routers/world-router.ts:187-412` — `createSession` procedure
- `apps/qino-world-backend/src/services/area-placement-service.ts` — `assignNativePlacements()`
- `apps/qino-world-backend/src/db/schema.ts:80-94` — sessions table

**Current state:**

`createSession` procedure:
1. Resolve scenario → get `templateId`
2. Query Journey for active arcs → build `arcContext` (`{ foregroundedArcId, activeArcs[] }`)
3. Create session: `userId`, `name`, `templateId`, `currentScenarioId`, `arcContext`
4. Get areas for template
5. Two placement paths:
   - **Ceremony path** (if `ceremonyId` provided): use transformation results from KV for placements
   - **Default path**: `assignNativePlacements()` using deck location hints → round-robin fallback
6. Populate facets from persistent `manifestationFacets` cache

Sessions table:
```sql
sessions: id, userId, name, currentScenarioId, templateId, arcContext (JSON)
```

`assignNativePlacements()` is a pure function: for each manifestation, tries exact match → contains match → round-robin on location hints from the deck.

### CYCLE CONTACT: Session Creation — Cycle Phase at Entry

At session creation, the system could compute the world's current cycle phase. Two approaches:

- **Record cycle phase at creation** — `cyclePhaseAtCreation` column on sessions. Useful for historical queries ("what phase was I in when I started?") but the phase keeps ticking after creation, so it's immediately stale.
- **Compute on-the-fly** — more aligned with the "computable, not stored" principle. The frontend requests current state, the server computes current phase from `cycleDefinition + epochAnchor + Date.now()`.

The **computed approach is preferred** — recording the phase at creation is only valuable as a timestamp annotation, like recording the weather when you arrive at a place. The world has moved on by the time you read it.

### CYCLE CONTACT: Manifestation Placement — Cycle-Aware

Currently `assignNativePlacements` uses static location hints. With temporal texture, placement could be cycle-aware:

- The Keeper is on the Bridge during shift change, but in the Cantina during quiet watch
- The xenobiologist is at the tide pools during low tide, in the lab during high tide

**Design decision:** Do manifestation positions change with the cycle? Or is the effect purely on dialogue quality (the Keeper speaks differently at different times, but stays on the Bridge)?

| Approach | Effect | Complexity |
|----------|--------|------------|
| **Static placement, dynamic dialogue** | Manifestation stays in one area; cycle affects how they speak, what they notice, what they mention | Low — only dialogue prompt changes |
| **Cycle-aware placement** | Manifestation moves between areas as cycle turns | High — needs a placement recalculation service, frontend polling, live area updates |
| **Hybrid: primary area + cycle wander** | Manifestation has a home area but occasionally appears elsewhere during specific phases | Medium — adds a probabilistic layer to placement |

**Recommendation for v1: Static placement, dynamic dialogue.** The cycle's effect on encounter quality is the primary texture. Physical movement is a v2 concern that adds significant runtime complexity (placement recalculation, frontend state management, movement animation). The "Keeper speaks differently during quiet watch" achieves 80% of the temporal texture value without touching placement infrastructure.

**Complexity: Low-Medium** — session creation itself needs minimal change if using computed-on-the-fly approach.

---

## 5. Crossing Threshold

**Files:**
- `apps/qino-world-backend/src/routers/world-router.ts:1493-1901` — `composeCrossing` procedure
- `apps/qino-world-backend/src/services/crossing-composition-service.ts` — episode generation
- `apps/qino-world-backend/src/services/transformation-service.ts` — foreign figure transformation
- `apps/qino-world-backend/src/services/voicing-service.ts` — native figure voicing
- `apps/qino-world-backend/src/services/ceremony-store.ts` — KV persistence

**Current state:**

`composeCrossing` orchestrates:
1. Fetch scenario + world
2. Classify manifestations: native (presentInWorlds includes this template) vs. foreign
3. **Native figures**: check facet cache → voice uncached figures (parallel)
4. **Foreign figures**: `transformForCrossing()` — generates worldName + facet + areaId (parallel)
5. Resolve native placements via `assignNativePlacements()` with deck location hints
6. `composeCrossingSequence()` — AI generates episode narrative from context
7. Build ceremony manifestations array with all placement + facet + image data
8. Store in KV with `ceremonyId`
9. Return episodes + manifestations + ceremony ID

**Context passed to AI composition:**
```typescript
{
  world: { name, description },
  scenario: { name, description, lore, currentTension, atmosphericTone, spatialCharacter },
  manifestations: [{ id, name, isNative, originName?, facet?, areaName?, origin?, personality? }],
  entryContext: "new-origin" | "re-entry"
}
```

### CYCLE CONTACT: Crossing Ceremony — Temporal Context

The crossing composition receives scenario context (what's happening) but **no cycle context** (when it's happening). With temporal texture, the ceremony should know:

- **Current cycle phase** — is the station in quiet watch or supply run arrival?
- **Phase quality** — what atmosphere does this moment carry?
- **How the crossing narrative should be colored** — arriving during the quiet watch is contemplative and slow; arriving during a supply run is bustling and chaotic

This is where the attunement facet's "tuning fork" meets temporal texture. The crossing strikes the tuning fork at a particular **temporal** frequency — not just the arc frequency, but the moment-in-time frequency.

**Implementation:** Add `temporalContext` to the composition input:
```typescript
temporalContext?: {
  cycleName: string;          // "shift-cycle"
  phaseName: string;          // "quiet-watch"
  phaseQuality: string;       // "contemplative, still, sounds carry further"
  phaseIndex: number;         // 0-3
  cycleProgress: number;      // 0.0-1.0 within current phase
}
```

The composition prompt would include this as temporal atmosphere, shaping how the AI writes the arrival episodes.

### CYCLE CONTACT: Voicing — Cycle-Aware Facets

The voicing service generates how a figure speaks in this world (their facet). Currently cycle-independent. The facet is generated once and cached in `manifestationFacets` table (keyed by `manifestationId + templateId`).

**Key insight:** Regenerating facets per cycle phase would be expensive and wasteful. The facet describes the figure's *general* adaptation to the world, not their moment-by-moment mood. The cycle's effect on dialogue quality is better expressed through the dialogue prompt's `=== THE MOMENT ===` section (see step 6 below) than through facet regeneration.

**Complexity: Low** — adding temporalContext to composition prompt is a prompt injection, not an architectural change.

---

## 6. Dialogue — Exploring the World

**Files:**
- `apps/qino-world-backend/src/services/dialogue-service.ts` — prompt building + generation
- `apps/qino-world-backend/src/routes/chat-stream.ts` — streaming endpoint (not a tRPC route — raw HTTP handler)

**Current state — system prompt structure:**

```
You are {figure.name}, in conversation with someone who just approached you.
Respond as yourself — not as a narrator describing a scene. This is dialogue, not prose.

=== THE SPACE ===
{area.name}
{area.description}

=== WHO YOU ARE ===
{figure.name}
{figure.origin}
{figure.personality}
{facet if voiced}

=== WHO'S HERE ===
{nearby figures with origin notes}

{universal constraints}
```

`DialogueContext` interface:
```typescript
interface DialogueContext {
  figure: Manifestation;
  area: Area;
  nearbyFigures: Manifestation[];
  recentMessages: Message[];
  userMessage: string;
  facet?: string | null;
}
```

The `generateDialogue()` function calls OpenRouter with `MODELS.DIALOGUE`, max 256 tokens, context label "dialogue".

### CYCLE CONTACT: Dialogue Prompt — THE PRIMARY SURFACE

**This is where temporal texture has its deepest effect.** The dialogue prompt needs a new layer between SPACE and IDENTITY:

```
=== THE MOMENT ===
{cyclePhase.name} — {cyclePhase.quality}
{how this manifestation relates to this phase}
{what's perceivable now that isn't at other times}
```

This is the "Cycle as Encounter Texture" from the temporal texture facet:
- The Keeper during quiet watch is contemplative, speaks slowly, notices things others miss
- The Keeper at shift change is brisk, distracted, focused on handover
- The Keeper when the supply run arrives is energized, telling stories about what came in
- The xenobiologist during tidal exposure speaks with authority, notices specimens
- The xenobiologist during high tide is reflective, theorizing, turning inward

**The cycle changes what's perceivable.** During quiet watch, the Keeper might mention sounds from the lower decks that are drowned out during busy hours. During a supply run, they reference a trader they haven't seen in three cycles.

**Implementation:**

`DialogueContext` gains a new field:
```typescript
interface DialogueContext {
  // ... existing fields ...
  temporalMoment?: {
    phaseName: string;          // "The Quiet Watch"
    phaseQuality: string;       // "Still. Sounds carry. The station breathes slowly."
    manifestationResonance?: string;  // "You are in your element — this is when you notice things"
    environmentalDetail?: string;     // "The lower deck hum is audible. Star-light pools on the deck plates."
  };
}
```

The `buildSystemPrompt()` function adds the `=== THE MOMENT ===` section when `temporalMoment` is present.

**Where does `temporalMoment` come from?** Computed at dialogue time:
1. Read world's cycle definition from DB (cacheable — it's static)
2. Compute current phase from `cycleDefinition + epochAnchor + Date.now()`
3. Look up manifestation's resonance for this phase
4. Look up area's modulation for this phase
5. Pass as `temporalMoment` to `DialogueContext`

This is a **service function** — a `TemporalService` or `CycleService` that computes the current moment given a world and optionally a manifestation + area.

**Complexity: Critical — but architecturally simple.** The change is a prompt injection (low) powered by a computation service (medium). No database writes, no state management, no async complexity. The hard part is the **authored content** — the cycle definitions and resonance mappings in the deck types that feed this computation.

---

## 7. Chat-Stream Endpoint

**Files:**
- `apps/qino-world-backend/src/routes/chat-stream.ts` — full streaming handler

**Current state:**

The chat-stream endpoint is the main dialogue handler (raw HTTP, not tRPC — needed for SSE streaming). Flow:

1. Validate + deduplicate
2. Load manifestation + placement + area
3. Detect three-way crossing (if activeCrossingId on placement)
4. Load world context (area, nearby manifestations)
5. **Layer 1 — Candidate Detection**: `detectMentionReferences()` string-matches user message against known mention names
6. Store user message in DB
7. Build system prompt (calls `buildSystemPrompt()` from dialogue-service)
8. Stream dialogue response
9. **Post-processing (finally block):**
   - Extract mentions from response (LLM call)
   - Filter against known manifestations/areas
   - Store in `messageMentions`
   - **Layer 2 — Substrate Tracking** (two-way only):
     - `trackRelationalInquiry()` for each elaborated mention
     - `checkAwakeningAvailability()` after each tracking
   - Record encounter in Journey + link to foregrounded arc

### CYCLE CONTACT: Chat-Stream — Temporal Context Loading

The chat-stream handler loads manifestation, placement, and area context before building the prompt. This is where cycle phase computation would be injected:

```
// Current: Load world context
const area = await getArea(placement.areaId);
const nearbyManifestations = await getNearby(session, placement);

// With cycles: Compute temporal moment
const cyclePhase = computeCurrentPhase(world.cycleDefinition, world.epochAnchor);
const temporalMoment = buildTemporalMoment(cyclePhase, manifestation, area);
```

The `temporalMoment` then flows into the `DialogueContext` passed to `buildSystemPrompt()`.

**Note:** The chat-stream handler doesn't currently load the world or its properties — it only loads the area and manifestation. Adding cycle context requires either:
- Loading the world's cycle definition (one extra DB read, but cacheable)
- Storing cycle definition on the session (snapshot at creation — but cycles change across scenarios)
- Computing from template + global config (avoiding per-request DB reads)

**Complexity: Low** — one additional read + pure computation.

---

## 8. Mention Extraction & Substrate Accumulation

**Files:**
- `apps/qino-world-backend/src/services/mention-extraction.ts` — entity extraction from dialogue
- `apps/qino-world-backend/src/services/substrate-service.ts` — substrate tracking + awakening check

**Current state — Mention Extraction:**

`extractMentions()` calls OpenRouter with structured output (Zod schema). Extracts: planets, places, artifacts, factions, persons. Also detects elaborations on known mentions with knowledge depth classification (intimate/adjacent/distant).

`filterMentionsAgainstManifestations()` removes mentions matching known entities (Jaro-Winkler 0.85 threshold).

**Current state — Substrate Tracking:**

`trackRelationalInquiry()`:
1. Get or create `mentionSubstrate` record (fuzzy-matched by name)
2. Determine knowledge depth (LLM-classified or heuristic fallback)
3. Create `relationalInquiries` record (perspective content preserved as compost)
4. Check awakening availability:
   - `minUniquePerspectives: 2` (at least 2 figures discussed it)
   - `requireIntimateKnowledge: true` (at least one intimate depth)
   - If met: `awakeningAvailable = true`

**Database tables:**
```sql
mentionSubstrate: id, sessionId, mentionName, mentionType, deepestKnowledge, awakeningAvailable, awakenedManifestationId
relationalInquiries: id, mentionSubstrateId, mentionMessageId, askedManifestationId, knowledgeDepth, responseMessageId, perspectiveContent
messages: id, sessionId, manifestationId, role, content, clientMessageId, actionMeta (JSON), areaId
```

### CYCLE CONTACT: Temporal Substrate

The "temporal substrate" open thread from the facet: should the system record *when in the cycle* an encounter happened?

Currently `messages` have `createdAt` timestamps but no cycle phase. `relationalInquiries` have `createdAt` but no temporal context.

**Adding cycle phase to encounter records:**

| Table | New Column | Value | Use |
|-------|-----------|-------|-----|
| `messages` | `cyclePhase` (text, nullable) | Phase name at message time | "You met the Keeper during the quiet watch" |
| `relationalInquiries` | `cyclePhase` (text, nullable) | Phase when inquiry happened | Temporal dimension in substrate |
| `mentionSubstrate` | (no change) | Substrate is aggregate | Phase lives on individual records |

This enables the "stories of what you missed" pattern — manifestations reference cycle-specific events from the past:

> "You should have been here during the last supply run. Keth traded three fuel cells for a single data crystal."

The cycle phase on messages lets the system build temporal narratives: what happened during which phase, what the user saw vs. missed.

**Complexity: Low** — new nullable columns, populated at write time from the same computation used for the dialogue prompt.

### CYCLE CONTACT: Awakening Staging — Temporal Gate

The awakening check (`checkAwakeningAvailability`) currently uses a pure substrate threshold. With temporal texture, awakening gains a **temporal gate**:

1. **Substrate gate** (existing): 2+ perspectives, 1+ intimate → `awakeningAvailable = true`
2. **Temporal gate** (new): wait for the right cycle phase → `awakeningReady = true`

"Lira, mentioned as someone who arrives with the traders, awakens **during the next supply run**."

The `mentionSubstrate` record would need:
- `awakeningAvailable: boolean` (existing — substrate threshold met)
- `awakeningPhaseAffinity: string | null` (new — which cycle phase this mention should awaken during)
- The awakening service checks both: substrate warm enough AND cycle in the right phase

The `awakeningPhaseAffinity` could be:
- Derived from the mention's context (LLM inference: "this person was mentioned in the context of trade arrivals")
- Derived from which manifestation gave the intimate perspective (if the Keeper mentioned them during quiet watch, maybe they awaken during quiet watch)
- Authored in the deck's crossing definitions (existing `DeckCrossing.trigger` field already hints at temporal conditions)

**Complexity: Medium** — adds a computation gate to the awakening check, plus inference or authoring of phase affinity.

---

## 9. Journey State Response

**Files:**
- `apps/qino-world-backend/src/routers/world-router.ts:481-638` — `getJourneyState` procedure

**Current state:**

The frontend requests world state via `getJourneyState({ sessionId, scenarioId? })`. Response:

```typescript
{
  session,
  currentScenario: { id, name, worldId, worldName, worldImageUrl },
  areas: AreaWithPosition[],  // areas with layout positions + placed manifestations
  canTransition: boolean,     // are there more scenarios?
  nextScenarios?: [{ id, name, description, orderIndex }]
}
```

### CYCLE CONTACT: Journey State — Current Cycle Phase

The journey state response needs to include the current cycle phase so the frontend can:
- Show visual indicators (lighting shifts, atmospheric effects)
- Display what's happening in the world right now
- Potentially show different area descriptions per phase
- Indicate upcoming phase transitions

```typescript
{
  // ... existing fields ...
  temporalState?: {
    currentPhase: {
      name: string;         // "The Quiet Watch"
      quality: string;      // "Still. Sounds carry."
      index: number;        // 2 of 4
    };
    nextPhase?: {
      name: string;         // "Shift Change"
      transitionsIn: number; // minutes until transition (real time)
    };
    cycle: {
      name: string;         // "Station Shifts"
      totalPhases: number;  // 4
    };
  }
}
```

### CYCLE CONTACT: Scenario Transition — Readiness Signals

Currently `canTransition` is a simple boolean (are there more scenarios?). With temporal texture and readiness lensing, this becomes richer:

The transition isn't available just because the next scenario exists — it's available when the readiness signals indicate the chapter is ripe. This connects to the Lens Lab assessment infrastructure:

- **Convergence**: are the scenario's threads drawing together?
- **Saturation**: has the scenario been explored?
- **Fertility**: is there material for what comes next?
- **Closure**: is the currentTension approaching resolution?

This would mean the journey state response includes readiness data:
```typescript
{
  canTransition: boolean,           // Existing: structural (more scenarios exist)
  transitionReadiness?: {           // New: lensed (substrate is warm enough)
    convergence: number;            // 0.0-1.0
    saturation: number;
    fertility: number;
    closure: number;
    isRipe: boolean;                // fertility high + closure rising
  }
}
```

**Complexity: Low (phase)** — pure computation from cycle definition. **High (readiness)** — requires a new service that reads substrate and computes readiness signals.

---

## 10. Summary: Contact Surface Map

| # | Layer | Current State | Cycle Contact | Complexity | Priority |
|---|-------|--------------|---------------|------------|----------|
| 1 | **Deck Types** | No temporal data | `DeckCycle` type with phases, resonance, modulation | Medium | **P0 — Foundation** |
| 2 | **Arc Deck Browser** | Static counts | Temporal character preview | Low | P3 — Polish |
| 3a | **World Table** | Static description | Cycle definition JSON + epoch anchor | Medium | **P0 — Foundation** |
| 3b | **Scenario Table** | Static tension/lore | Active cycle modifiers, transition conditions | Medium | P1 — Enhancement |
| 4a | **Session Creation** | Arc context only | Cycle phase computed on-the-fly | Low | P2 — Runtime |
| 4b | **Manifestation Placement** | Static location hints | Cycle-aware placement (v2 concern) | High | P3 — Future |
| 5 | **Crossing Ceremony** | Scenario context only | `temporalContext` in composition prompt | Low | P1 — Enhancement |
| 6 | **Dialogue Prompt** | Space + Identity | **`=== THE MOMENT ===`** cycle phase layer | **Critical** | **P0 — Primary texture** |
| 7 | **Chat-Stream** | No temporal loading | Load cycle definition, compute phase | Low | P0 — Supports #6 |
| 8a | **Messages Table** | `createdAt` only | `cyclePhase` column (nullable) | Low | P1 — Enhancement |
| 8b | **Substrate Tracking** | Pure threshold | Temporal gate on awakening | Medium | P1 — Enhancement |
| 9a | **Journey State** | Static areas | Current cycle phase in response | Low | P0 — Frontend needs it |
| 9b | **Scenario Transition** | Boolean `canTransition` | Readiness signals from lensing | High | P2 — Future |

---

## 11. Architectural Findings

### Finding 1: Cycle Phase Is Computable, Not Stored

Since world time ticks independently in real time, the current phase is always derivable:
```
currentPhase = computePhase(cycleDefinition, epochAnchor, Date.now())
```

The database stores the **definition** (authored) + **epoch anchor** (when counting began). The **state** (current position) is pure computation. This means:
- No cron jobs or background workers
- No state synchronization across service instances
- No "ticking" mechanism — the world is always exactly where it should be
- Phase computation is a pure function: deterministic, testable, cacheable

### Finding 2: Two Distinct Data Layers

The **authored layer** (design time):
- Cycle names, phase qualities, durations
- Manifestation resonance (who comes alive when)
- Area modulation (how places change)
- Mention affinity (which phases are introduction windows)

Lives in: starter deck types → seeded into DB

The **computed layer** (runtime):
- Current phase position
- What's perceivable now
- Who's most resonant
- Temporal context for dialogue prompt

Lives in: a service function (`CycleService` or `TemporalService`)

### Finding 3: The Dialogue Prompt Is the Primary Surface

Everything else is infrastructure supporting the moment where the cycle phase enters the conversation. The `=== THE MOMENT ===` prompt section is where the user **feels** temporal texture. Everything upstream (deck definitions, seeding, session creation) feeds into this single injection point.

The system prompt's layered structure (`SPACE → MOMENT → IDENTITY → RELATIONS → CONSTRAINTS`) mirrors the priority: the space sets the scene, the moment sets the atmosphere, the identity speaks through both.

### Finding 4: Static Placement, Dynamic Dialogue (v1)

Manifestation positions should remain static in v1. The cycle's effect flows through **dialogue quality**, not **physical movement**. This achieves 80% of temporal texture value with minimal infrastructure change:

- No placement recalculation service
- No frontend polling for position changes
- No movement animation or area transition handling
- The Keeper speaks differently during quiet watch, but you find them on the Bridge

Physical movement (cycle-aware placement) is a v2 concern.

### Finding 5: The Minimal Viable Path

To get temporal texture working end-to-end, only three things are strictly required:

1. **Deck types gain cycle definitions** — what cycles, what phases, what qualities
2. **World table stores cycle definition + epoch** — seeded from deck
3. **Dialogue prompt gains `=== THE MOMENT ===`** — computed at dialogue time from definition + now

Everything else (ceremony context, temporal substrate, awakening gates, readiness signals, journey state phase) enriches the experience but isn't required for the core texture to be felt in dialogue.
