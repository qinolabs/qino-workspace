# Lens Ecosystem Integration — Terrain

## I. The Nervous System Discovery (What This Session Proved)

The architectural boundary that shapes everything in this territory. Understanding this is prerequisite.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Ecosystem calibration concept** | `qino-concepts/concepts/tech-qino-lens/content/ecosystem-calibration.md` | The nervous system metaphor: Journey carries sensitivity, organs generate experience. Validated by building. Contains the vocabulary-to-grammar framing. |
| **Validation learnings** | Same file, "Validation Learnings" section (2026-02-09) | The two builds that revealed the boundary: Walk forced its data into `userNote`, proving the runtime API conflates ecosystem and local operations. Walk wiring was removed. |
| **Infra 09 full status** | `implementations/qino-infra/content/09-ecosystem-calibration.md`, "Runtime API Removal" section | Runtime API surface removed (2026-02-10) — zero consumers across all modalities. Calibration vocabulary retained (SlotType, selectLens cascade, LensContext). |
| **Design principles** | `qino-concepts/concepts/ecosystem-design-principles/content/design-principles.md` | "Trust the Ecosystem" and "Boundaries of Meaning" — the two principles this boundary instantiates. Journey provides intelligence; modalities own their experience. |

**The pattern**: Journey's existing `voiceFigureWithData` handles crossing voicing. For narrator, companion, soundscape, inhabiting (modality-local), modalities generate their own content. The nervous system operates through **calibration propagation** (lens versions → cache invalidation → different perception), not runtime API calls.

---

## II. Journey Infrastructure (What Exists)

The nervous system's current wiring — what's built, what's proven, what's ready for extension.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Voicing service** | `qino-infra/journey/src/services/voicing-service.ts` | Core pipeline: `voiceFigureWithData`, `buildVoicingPrompt`, `selectLens` (7-step cascade), substrate fingerprinting. The enhanced `selectLens` considers figure composite, slot defaults, layer preferences. |
| **Journey schema** | `qino-infra/journey/src/db/schema.ts` | Tables: `lenses` (24 seeded), `facets` (with `slotType` column), `encounters`, `substrates`, `arcs`. |
| **Journey RPC interface** | `qino-infra/journey/rpc.ts` | `voiceFigureWithData`, `voiceFigureMultipleLensesWithData`, `voiceWithCustomPrompt`, `getLenses`, `seedLenses`, plus encounter and embedding methods. |
| **Slot type vocabulary** | `qino-infra/journey/src/types.ts` | 8 `SlotType` values: crossing, theme, inhabiting, narrator, companion, soundscape, assessment, current. `SLOT_DEFINITIONS` constant with descriptions. Design vocabulary for the `selectLens` cascade. |
| **LensContext extensions** | `qino-infra/journey/src/types.ts` | `slotType`, `figureComposite`, `modalityProfile` fields on `LensContext` — extension points that inform lens selection without requiring a separate runtime API. |

**What's proven**: Crossing voicing through `voiceFigureWithData` works. The 7-step lens selection cascade handles optional slot/profile context gracefully. 24 lenses with version-based cache invalidation provide the calibration mechanism.

**What was removed (2026-02-10)**: The Infra 09 runtime API surface — 8 RPC methods, 2 DB tables (`modality_profiles`, `attention_adaptations`), `modality-profile-service.ts`, and ~440 lines of slot-aware voicing code. Zero consumers across all modality apps. See `implementations/qino-infra/content/09-ecosystem-calibration.md`, "Runtime API Removal" section.

**The reframing**: The nervous system metaphor was correct but the implementation model was wrong. Journey's nervous system operates through **calibration propagation** (lens version bumps → cache invalidation → different perception), not through runtime API calls from modalities. The existing voicing pipeline is the working nervous system.

---

## III. World Integration (The Working Consumer + Next Steps)

World is 23 iterations deep. It has the richest infrastructure for lens integration and the one proven integration point (crossing).

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **World voicing service** | `apps/qino-world-backend/src/services/voicing-service.ts` | Local facet generation for crossing. Builds prompts from manifestation + world + scenario context. |
| **Transformation service** | `apps/qino-world-backend/src/services/transformation-service.ts` | Unified AI call: `worldName + facet + areaId` during crossing ceremony. The crossing integration that `voiceFigureWithData` feeds into. |
| **Crossing composition** | `apps/qino-world-backend/src/services/crossing-composition-service.ts` | Orchestrates crossing: resolution via Weave, voicing via Journey, placement via areas. The full pipeline from ecosystem figure to World manifestation. |
| **Awakening service** | `apps/qino-world-backend/src/services/awakening-service.ts` | Mention-to-manifestation pipeline. Readiness lens integration site (assessment slot). Substrate now accumulates via natural mention detection — the pipeline is live. |
| **Natural mention detection** | `apps/qino-world-backend/src/routes/chat-stream.ts`, `src/services/mention-extraction.ts`, `src/utils/entity-similarity.ts` | Two-layer detection: Layer 1 (Jaro-Winkler similarity against known mention names) catches exact and near-exact references at zero inference cost. Layer 2 (extraction piggyback) detects referenced mentions during the same LLM call that extracts new ones. This is the live entry point for substrate accumulation — users naturally reference mentions in conversation, and the system detects it. Built 2026-02-16 (7ac0714). |
| **World prompt builder** | `apps/qino-world-backend/src/services/world-prompt-builder.ts` | Builds dialogue context from area + nearby figures. Future site for narrator slot integration — figures present should shape narrative voice. |
| **Iteration 23 (inline crossing)** | `implementations/qino-world/content/23-inline-crossing-transformation.md` | Next World iteration. Wires transformation into inline crossing path. Uses `voiceFigureWithData` for crossing voicing. |

**What World needs from a future lens intelligence API**:
- **Narrator slot**: Which lens to use when building dialogue context. World prompt builder would inject lens-colored framing.
- **Theme slot**: How a scenario resonates through a lens. Scenario-level atmospheric coloring.
- **Inhabiting slot**: How a figure's lens pattern shapes their presence in a specific scenario.

---

## IV. Walk Integration (The Conceptual Consumer)

Walk is concept-rich but infrastructure-lean on lens integration. The validation proved Walk's data doesn't fit Journey's substrate shape — Walk generates its own content.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Walk story** | `implementations/qino-walk/story.md` | Three-mode architecture (Topic, Story, Soundscape). Key insight: "No figure addresses the walker." Figures color the narrator's awareness, not direct speech. Fundamentally different slot behavior than World's crossing. |
| **Walk iteration 16** | `implementations/qino-walk/content/16-world-soundscape-integration.md` | Phases 1-3 complete (World + Listener model). Phases 4-5 pending: planning layer (listener's path through world) and mention seeds. |
| **Walk ecosystem integration** | Walk implementation content | Design decisions: figureRef hybrid format, simplified substrate model, ecosystem-led lens selection. Walk provides context, Journey selects lens. |

**What Walk needs from a future lens intelligence API**:
- **Companion slot**: Which lens for topic-walk voice. Walk generates the dialogue, informed by Journey's lens selection + attention adaptations for embodied attention.
- **Soundscape slot**: How figure qualities translate to audio parameters. Journey provides lens data; Walk translates to soundscape params.
- **Narrator slot**: How figures present color the narrator's awareness. Same metadata API, different consumption pattern than World.

**The key Walk constraint**: "No figure addresses the walker." Walk's lens integration is indirect — figures don't speak through lenses, they color the narrator's attention through their embodied qualities.

---

## V. Lens Lab (The Laboratory Surface)

Where the calibration ecology lives. Lens Lab measures, compares, experiments with how lenses behave.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Lens Lab story** | `implementations/lens-lab/story.md` | Workshop of workshops. 186 tests. Phase 6 (Emergence Integration) active. |
| **Iteration 22 (compound profile)** | `implementations/lens-lab/content/22-compound-profile-and-discovery.md` | Compound profile view, anomaly surfacing, cross-grammar experiments. Complete. |
| **Iteration 23 (relational + crossing)** | `implementations/lens-lab/content/23-relational-lenses-and-crossing.md` | Relational lenses (coherence, proximity), world fixtures, crossing simulation. Pending. |

**Lens Lab's role in the calibration loop**:
1. Lab reads lens definitions and voicing behavior from Journey
2. Lab measures lens behavior against real and fixture substrate
3. Lab surfaces anomalies and cross-grammar discoveries
4. (Future) Lab influences lens calibration — the mechanism for this is an open question now that the adaptation tables were removed

The read path is built. The write path (Lab calibrating Journey's lens behavior) needs a new mechanism — the removed `attention_adaptations` table was one approach, but it had zero consumers. The feedback loop design should emerge from actual Lab experiments, not speculation.

---

## VI. Concept Architecture (The Design Language)

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Core lens concept** | `qino-concepts/concepts/tech-qino-lens/content/concept.md` | Bidirectional gaze, scope vs slot, emergence layer, anti-fixation. The foundational document. |
| **Lens slots and pipelines** | `qino-concepts/concepts/tech-qino-lens/content/lens-slots-and-pipelines.md` | Complete slot taxonomy, pipeline composition. "Lenses don't scale by adding more lenses. They scale by recognizing more slots." |
| **Lenses, manifestations, figures** | `qino-concepts/concepts/tech-qino-lens/content/lenses-manifestations-figures.md` | Recursive architecture: manifestations as composite lenses. Three flows: lens voices manifestation, manifestation embodies lens, manifestation voices other substrate. |
| **Ecosystem calibration** | `qino-concepts/concepts/tech-qino-lens/content/ecosystem-calibration.md` | Nervous system metaphor, vocabulary-to-grammar, dialect vs replacement, calibration ecology. Updated with validation learnings. |

---

## VII. Simulation as Slot Orchestration

The discovery that shifts the ecosystem from tool to agentic system. Simulation prototyping applies the same distributed ownership pattern as `seed()` / `seedFromDeck()` — each modality owns its simulation piece.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Simulation prototyping plan** | `implementations/docs/content/simulation-architecture.md` | The distributed orchestration pattern: Lens Lab orchestrates, World/Walk/Journey execute domain simulation. UserBehaviorProfile model, MCP findings channel. |
| **Simulation operator agent** | `.claude/agents/simulation-operator.md` | Methodology home: experiment design, orchestration pattern, human integration protocol, built-in experiment templates. |
| **Starter deck adapter** | `apps/lens-lab-backend/src/adapters/starter-deck-adapter.ts` | Transforms deck types → assessment-compatible substrate. `deckScenarioToSubstrate()`, `deckWorldToContext()`, `listDeckSummaries()`. |
| **Simulation router** | `apps/lens-lab-backend/src/routers/simulation-router.ts` | 8 public tRPC procedures across two phases: deck browsing + assessment (Phase 1), user session simulation + profile comparison (Phase 2). Development-time tool, no auth. |
| **Simulation profiles** | `apps/lens-lab-backend/src/simulation-profiles.ts` | 4 built-in profiles (curious-explorer, lore-seeker, relationship-builder, speedrunner) spanning the engagement spectrum. Profiles are a Lens Lab concern — World receives only the `profilePrompt` string. |
| **World simulation service** | `apps/qino-world-backend/src/services/simulation-service.ts` | Turn loop using the real dialogue pipeline: user message generation → `generateDialogue` → `extractMentions`. Same code path as production chat-stream. |
| **Simulation framework docs** | `implementations/lens-lab/content/24-simulation-framework.md` | Retrospective documentation of the built pipeline + forward work: `simulateModelComparison` and `simulateMatrix` endpoints. |
| **Starter decks package** | `packages/starter-decks/` | 13 decks with worlds, scenarios, manifestations, crossings. The default substrate for all simulation. |

**The distributed simulation architecture**: Each modality owns its simulation piece. World simulates dialogue, crossing, and awakening. Walk simulates atmosphere and companions. Lens Lab orchestrates and assesses. This mirrors `seed()` ownership — World knows how to seed World data, so World knows how to simulate World interactions.

**Modalities as outer lenses**: The slot architecture that governs inner lens behavior (crossing, narrator, companion, soundscape) now describes how humans perceive the system through modality surfaces. Each modality is a perception slot — a lensing surface for human experience. World lenses through dialogue and place. Walk lenses through movement and atmosphere. This reframing makes the modality–slot relationship recursive: lenses operate inside modalities, and modalities operate as lenses on the ecosystem.

**The agentic framing**: Simulation prototyping turns the ecosystem into an agentic system. Claude orchestrates through Lens Lab, each modality responds with its domain expertise. The pattern mirrors how lens slots work internally: orchestration selects, slots generate. The simulation operator agent codifies the methodology — hypothesis → substrate selection → simulation → assessment → findings.

**Starter decks as default substrate**: 13 decks provide the testing ground. Side effects: deck quality improves through simulation feedback, we learn what makes good deck design, different themes surface different emergence patterns. The threshold deck's frontier-station scenario becomes the canonical test case.

**User behavior profiles**: Different simulated users (curious explorer, lore-seeker, relationship-builder, speedrunner) produce different substrate patterns — testing how the system responds to the full range of human engagement. The key test: same deck scenario, 4 profiles → 4 substrates → 4 readiness assessments.

---

## VIII. Dialogue Quality + Model Economics

Dialogue quality became measurable through the simulation pipeline. The prompt architecture was reworked and validated across models, establishing a cross-cutting quality surface that connects World (prompt owner), Lens Lab (test harness), and Walk (future consumer).

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Dialogue quality navigator** | `navigators/dialogue-quality.md` | Owns the cross-cutting quality surface: prompt architecture, model comparison findings, model tier table, mention extraction quality. This navigator references it rather than duplicating. |
| **World iteration 24** | `implementations/qino-world/content/24-dialogue-voice-quality.md` | Prompt rework: replaced keyword-matching pipeline with direct personality injection, conversation-mode framing, relaxed formatting rules. Also documents natural mention detection (Layer 1 + Layer 2) and the dead code discovery that motivated it. |
| **AI client** | `packages/ai/` (`@qinolabs/ai`) | Centralized model tier architecture. The current 4-tier system (CREATIVE, ANALYTICAL, DIALOGUE, STRUCTURED) was consolidated on 2026-02-16 from a previous 6-tier layout. Lens-lab and emergence-lab assessment services migrated from hardcoded raw-fetch to `@qinolabs/ai` with ANALYTICAL tier (GPT-4.1 Mini, $0.40/$1.60). |

Model tier architecture is tracked in the dialogue-quality navigator (`navigators/dialogue-quality.md` SS AI Client Model Tiers). The current 4-tier system (CREATIVE, ANALYTICAL, DIALOGUE, STRUCTURED) was consolidated on 2026-02-16 — FAST, CHAT, and STRUCTURED_PREMIUM removed (zero consumers after migration). The key outcome for this territory: assessment services across lens-lab and emergence-lab now run through `@qinolabs/ai` with the ANALYTICAL tier at 75-97% cost reduction versus the previous hardcoded Sonnet 4 calls.

**What the prompt rework proved**: Rich character data (origin, personality, visual presence from starter decks) flows directly to the LLM. The LLM infers voice from character — no keyword-matching intermediary. The baseline is model-agnostic: all 4 tested models produced character-differentiated dialogue with deck-specific tone.

**What this unlocks across the territory**:
- **Simulation as regression tool**: Any prompt change or model swap validated against the 13-deck baseline in minutes. The `dialogueModel` override threads through the entire pipeline.
- **Walk prompt architecture**: The "pass rich data, let LLM infer" pattern directly informs Walk's narrator/companion voice design. Walk's indirect consumption (figures color narrator awareness) is easier when prompts don't prescribe voice buckets.
- **Crossing dialogue quality**: World 23 (inline crossing) inherits improved character voice. Witness reactions gain texture when the arriving figure speaks distinctively.
- **Deck quality as observable property**: Different decks produce measurably different substrate density, mention extraction rates, and readiness profiles. The threshold deck's frontier-station is the reference; other decks compare against it.
- **Assessment cost reduction**: Lens-lab and emergence-lab assessment services running through ANALYTICAL (GPT-4.1 Mini) instead of hardcoded Sonnet 4 makes calibration experiments cheaper to run at scale.

---

## Reading Order

For entering this territory fresh:

1. **The nervous system metaphor** -> `qino-concepts/concepts/tech-qino-lens/content/ecosystem-calibration.md` (first 3 sections) — understand vocabulary vs grammar, the dialect principle, sensitivity allocation
2. **The validation discovery** -> same file, "Validation Learnings" section — understand why Journey generates for ecosystem ops and modalities generate for local ops
3. **The core lens concept** -> `qino-concepts/concepts/tech-qino-lens/content/concept.md` (sections 1-2) — bidirectional gaze, scope vs slot
4. **What was built and simplified** -> `implementations/qino-infra/content/09-ecosystem-calibration.md` — the iteration that built infrastructure, then removed the runtime surface; read goals + "Runtime API Removal" section
5. **The slot taxonomy** -> `qino-concepts/concepts/tech-qino-lens/content/lens-slots-and-pipelines.md` (Slot Architecture section) — understand where lenses operate
6. **World's readiness** -> `implementations/qino-world/story.md` + `content/23-inline-crossing-transformation.md` — the working consumer and its next step
7. **Walk's constraint** -> `implementations/qino-walk/story.md` — "No figure addresses the walker" and the three-mode architecture
8. **The laboratory** -> `implementations/lens-lab/story.md` + `content/23-relational-lenses-and-crossing.md` — where calibration happens
9. **Design principles** -> `qino-concepts/concepts/ecosystem-design-principles/content/design-principles.md` — Trust the Ecosystem, Boundaries of Meaning
10. **Simulation as slot orchestration** -> § VII above + `.claude/agents/simulation-operator.md` — the agentic framing: distributed simulation, modalities as outer lenses, starter decks as default substrate, user behavior profiles
11. **Dialogue quality and model economics** -> § VIII above + `implementations/dialogue-quality/story.md` — prompt architecture review, model comparison matrix, the cross-cutting quality surface connecting World, Lens Lab, and Walk

---

## Open Questions

### Lens Intelligence Beyond Crossing

- Does a metadata-only API (`getLensContext`) still make sense now that the runtime surface was removed? Or should modalities simply use `selectLens` knowledge embedded in their own prompt-building code? *(2026-02-16: natural mention detection provides substrate accumulation without requiring a metadata API — the existing pipeline may be sufficient for the assessment slot without new infrastructure.)*
- What's the first modality slot beyond crossing that needs lens awareness? Narrator (session voice) vs theme (scenario atmosphere) vs companion (walk dialogue)? *(2026-02-16: assessment slot moved up — natural mention detection means substrate now accumulates during normal conversation, making lens assessment of post-dialogue state a live possibility rather than a theoretical one.)*
- Should lens intelligence be delivered through the existing `LensContext` type (already has `slotType`, `figureComposite`, `modalityProfile` fields) rather than a new RPC method?

### Modality Consumption Patterns

- How does Walk's "no figure addresses the walker" principle translate into lens integration code? The lens intelligence colors the narrator, not the figure. What does the prompt structure look like?
- When a modality needs lens-informed generation, does it call Journey at all, or does it just use the lens definitions (already available via `getLenses`) and build its own prompts?

### Simulation & Agentic Architecture

- How does Walk's atmosphere simulation feed back into deck design? Walk owns its simulation piece but the orchestrator (Lens Lab) needs to interpret results and surface findings. What's the interface between Walk simulation output and deck quality assessment?
- What's the right granularity for user behavior profiles? 4 built-in archetypes (curious-explorer, lore-seeker, relationship-builder, speedrunner) vs. continuous parameter space. Archetypes are more interpretable; parameters are more flexible.
- When does the starter deck generator graduate from prototyping tool to production feature? The assessment pipeline becomes the quality function for generated decks, but at what point does generated content meet the bar set by hand-designed decks?

### Dialogue Quality & Model Selection

- Does the conversation-mode framing change behavior over longer sessions (15+ turns)? The matrix tested 6 turns — arc awareness may need validation at depth.
- How does the `speedrunner` profile interact with relaxed formatting? Ultra-compressed responses (7-36w from Grok) are viable dialogue, but do they produce enough mention substrate for readiness assessment?
- When DeepSeek V4 arrives, does the model comparison matrix need to expand beyond 3 decks? Current selection (threshold, night-train, tide-pool) covers sci-fi/European/ecological themes but misses others.
- Walk's narrator voice needs a model tier — does DIALOGUE (DeepSeek) suit indirect figure-coloring, or does the narrator's longer-form output need a different model?

### Calibration Loop

- What mechanism replaces `attention_adaptations` for Lab-driven calibration? Lens version bumps? Prompt template updates? Something lighter?
- How do Lens Lab cross-grammar experiments feed back into lens behavior?
- Does the calibration loop need infrastructure, or does it happen through human-mediated lens definition updates?

---

## Session Log

### 2026-02-09 — Session 1: Navigator Created from Validation Momentum

**What happened**: Built Infra 09 (Ecosystem Calibration), ran two validation builds revealing the nervous system boundary, then mapped the full territory of lens ecosystem integration across Journey, World, Walk, Lens Lab, and the concept architecture.

**Key moves**:
- Discovered the architectural boundary: Journey generates for ecosystem ops (crossing, assessment); modalities generate for local ops (companion, narrator, soundscape) using Journey's lens intelligence as metadata
- Identified three independent tracks converging toward the lens intelligence API
- Located the missing piece: `getLensContext` — metadata-only RPC enabling modality-local lens consumption
- Distinguished this navigator from emergence-experiments: sensitivity routing vs emergence mechanics

**What was produced**:
- Infra 09 post-validation corrections (committed: voicing-service optional profile lookup, simplified seed data, documentation)
- Ecosystem calibration concept updated with validation learnings
- This navigator node in the workspace graph

**What shifted**: The lens ecosystem territory crystallized from diffuse "lens integration" into a specific architectural pattern — nervous system (Journey metadata API) feeding organs (modality-local prompt building). The metaphor went from concept to code to architectural constraint.

**What remains alive**:
- `getLensContext` API design — return type, caching, RPC shape
- Which World slot activates first
- Walk's indirect consumption pattern needs concrete prompt design
- Lens Lab iteration 23 as independent calibration progress
- The feedback loop: when does Lab start writing adaptations back to Journey?

### 2026-02-10 — Session 2: Runtime API Removal + Reframing

**What happened**: Grep confirmed zero consumers of all 8 Infra 09 RPC methods across every modality app. Removed the entire runtime API surface: 8 RPC methods, 2 DB tables, 1 service file, ~440 lines of slot-aware voicing code. Also removed `voicingTone` from lenses (decorative metadata never read by `buildVoicingPrompt`) and renamed `figureName` → `manifestationName` in voicing input types.

**The reframing**: The nervous system metaphor survived but the implementation model flipped. Journey's nervous system operates through **calibration propagation** — lens version bumps invalidate cached facets, causing re-voicing with potentially different perception. This is already working. The runtime API surface (`voiceForSlot`, modality profiles, attention adaptations) was premature infrastructure for a consumption pattern that hadn't emerged and may never emerge in the form originally designed.

**What was retained**: The calibration vocabulary (`SlotType`, `SLOT_DEFINITIONS`, `LensContext` extensions, enhanced `selectLens` cascade) stays as design language that informs lens selection without requiring runtime API calls.

**What shifted**: The open questions changed character. "What should `getLensContext` return?" becomes "Does a metadata API make sense, or should modalities just use lens definitions directly?" The feedback loop question shifts from "how does Lab write to `attention_adaptations`?" to "what mechanism replaces it?"

### 2026-02-15 — Session 3: Simulation Prototyping Surface Design + Build

**What happened**: Designed the simulation prototyping surface as a distributed architecture — each modality owns its simulation piece, Lens Lab orchestrates and assesses. Discovered that this mirrors the slot architecture at a higher level: modalities as outer lenses, simulation as slot orchestration. Built Phase 1 (deck access + unencountered assessment MVP).

**Key moves**:
- Game-design-prototyping inspiration → starter deck access as default substrate
- User behavior profiles as the testing dimension (not just different decks, but different users)
- Distributed modality ownership: World simulates dialogue, Walk simulates atmosphere
- Arrived at "modalities as outer lenses" — the slot architecture goes recursive
- Built: starter deck adapter, simulation router, simulation-operator agent definition

**What was produced**:
- Terrain section VII (simulation as slot orchestration)
- Simulation-operator agent definition (`.claude/agents/simulation-operator.md`)
- Starter deck adapter (`apps/lens-lab-backend/src/adapters/starter-deck-adapter.ts`)
- Simulation router with public procedures for deck browsing and assessment
- Unit tests validating all 13 decks produce valid substrate shapes
- Phase 2: World backend `simulateSession()` RPC — dialogue simulation with mention extraction
- Phase 2: World simulation service (`apps/qino-world-backend/src/services/simulation-service.ts`) — turn loop, LLM user message generation, real dialogue pipeline
- Phase 2: 4 simulation profiles (curious-explorer, lore-seeker, relationship-builder, speedrunner) in `apps/lens-lab-backend/src/simulation-profiles.ts`
- Phase 2: Simulation router additions — `listProfiles`, `simulateUserSession`, `simulateProfileComparison`
- 277 unit tests passing (including 22 new profile tests)

**What shifted**: The lens ecosystem territory expanded from "how lenses reach modalities" to "modalities as perception slots in an agentic system." Simulation prototyping becomes the development practice upstream of all app work — test what works before building the full thing. The full loop is now operational: Claude can simulate different user types against any of the 13 starter decks and assess the resulting substrate through readiness lenses.

**What remains alive**:
- Phase 3: Crossing + dialogue simulation (World + Walk), atmosphere simulation
- MCP findings channel: surface simulation results through qinolabs-mcp
- Iteration 23 alignment: crossing simulation feeds directly into inline crossing work
- First real simulation run: test threshold deck with all 4 profiles

### 2026-02-15 — Session 4: Dialogue Quality + Model Economics

**What happened**: Reworked the dialogue prompt pipeline to replace keyword-matching with direct personality injection. Ran a 4-model x 3-deck x 6-turn simulation matrix (216 LLM calls) to validate the prompt baseline as model-agnostic. Selected DeepSeek V3.2 as the MODELS.DIALOGUE tier. Documented the simulation framework retrospectively (lens-lab iteration 24) and established the dialogue-quality cross-cutting workspace.

**Key moves**:
- Prompt rework: direct personality injection, conversation-mode framing, relaxed formatting rules — eliminated the 6-bucket keyword matching that discarded rich starter deck character data
- Model comparison matrix: 4 models (Haiku 4.5, DeepSeek V3-0324, Grok 4.1 Fast, Grok 4 Fast) x 3 decks (threshold, night-train, tide-pool) x 6 turns — all produced character-differentiated dialogue, confirming model-agnostic prompt baseline
- DeepSeek V3.2 deployed as MODELS.DIALOGUE: $0.25/$0.38 per M tokens (75-92% cheaper than Haiku), best quality-per-dollar balance, used by dialogue-service and awakening-service
- Simulation framework documented: lens-lab iteration 24 captures the full pipeline (deck adapter, simulation router with 8 procedures, 4 profiles, world-backend simulation service) and plans `simulateModelComparison` / `simulateMatrix` endpoints
- Cross-cutting dialogue-quality workspace (`implementations/dialogue-quality/`) established — connects World (prompt owner), Lens Lab (test harness), Walk (future consumer)

**What was produced**:
- World iteration 24 (dialogue voice quality) — prompt rework complete
- Dialogue quality workspace (`implementations/dialogue-quality/story.md`) with prompt review + model comparison findings
- DeepSeek V3.2 integration in `@qinolabs/ai` (new MODELS.DIALOGUE tier)
- Lens Lab iteration 24 (simulation framework documentation + forward work spec)
- Terrain section VIII (dialogue quality + model economics) added to this navigator
- Proposal annotation: test DeepSeek V4 when it lands (expected Feb 17)

**What shifted**: Dialogue quality is now measurable through the simulation pipeline — not just subjective play-testing. Model economics transformed: high-volume simulation sweeps became routine rather than expensive. The prompt architecture proved model-agnostic, meaning model selection is purely an economics and quality-ceiling decision, not a compatibility constraint. The cross-cutting dialogue-quality workspace makes prompt evolution visible across World, Lens Lab, and Walk.

**What remains alive**:
- World 23 (inline crossing transformation) — improved character voice makes witness reactions sharper
- Lens Lab 23 (relational lenses + crossing simulation) — can now pull live post-dialogue substrate from the simulation pipeline
- Lens Lab 24 forward work — `simulateModelComparison` and `simulateMatrix` endpoints replace bash-script-calling-curl
- Walk prompt architecture — "pass rich data, let LLM infer" pattern directly applicable to narrator/companion voice
- DeepSeek V4 evaluation — proposal annotation tracks this; one simulation run produces direct comparison
- Mention and awakening system assessment — do simulation profiles produce enough substrate for readiness assessment at different engagement styles?

### 2026-02-16 — Session 5: Natural Mention Detection + Model Tier Consolidation

**What happened**: Built the natural mention detection pipeline (Layer 1 Jaro-Winkler + Layer 2 extraction piggyback) that provides the live entry point for substrate accumulation. Consolidated the model tier architecture from 6 tiers to 4, migrating lens-lab and emergence-lab assessment services from hardcoded raw-fetch Sonnet 4 to `@qinolabs/ai` with the new ANALYTICAL tier. Switched live dialogue from Claude Sonnet 4.5 to DeepSeek V3.2.

**Key moves**:
- Natural mention detection: two-layer design where Layer 1 (Jaro-Winkler similarity) catches exact/near-exact references at zero inference cost, and Layer 2 (extraction piggyback) detects referenced mentions during the same LLM call that extracts new ones — zero additional inference. Resolves the dead code discovery from iteration 24: the substrate accumulation pipeline (`trackRelationalInquiry`, `mentionSubstrate`, `relationalInquiries`, `checkAwakeningAvailability`) now has a live entry point through natural conversation.
- Model tier consolidation: FAST, CHAT, STRUCTURED_PREMIUM removed (zero consumers after migration). ANALYTICAL added (GPT-4.1 Mini, $0.40/$1.60) for evaluation and assessment workloads. Lens-lab and emergence-lab assessment services migrated from hardcoded `fetch()` with raw Sonnet 4 model strings to `@qinolabs/ai`'s `generateStructured` with `MODELS.ANALYTICAL` — 75-97% cost reduction.
- Live dialogue model switch: `chat-stream.ts` and `three-way-dialogue-service.ts` moved from Claude Sonnet 4.5 to DeepSeek V3.2 (MODELS.DIALOGUE), extending the simulation-validated model to production traffic.
- Integration tests: 12 tests for mention extraction with real AI inference, validating the Layer 1 + Layer 2 pipeline end-to-end.

**What was produced**:
- Natural mention detection implementation (7ac0714): `entity-similarity.ts` (Jaro-Winkler), updated `mention-extraction.ts` (Layer 2 piggyback), updated `chat-stream.ts` and `simulation-service.ts` (detection integration)
- Mention detection unit tests (c54280a): 177 entity similarity tests, expanded mention extraction tests
- Integration tests (a2e8bd9): 12 tests with real AI inference validating the full extraction + detection pipeline
- Model tier consolidation (13a1804): 4-tier `MODELS` constant, 20 files updated across 7 apps and 2 infra packages
- Live dialogue switch (1dbb98a): production chat on DeepSeek V3.2

**What shifted**: The substrate accumulation pipeline went from dead code to live infrastructure. The mention-to-substrate-to-awakening chain now has its input: users reference mentions naturally in conversation, the system detects it, substrate accumulates, and the awakening check can eventually fire. This is the precondition for testing whether lens assessments diverge when different user profiles produce different substrate — the Phase 2 hypothesis from annotation 001.

The model tier cleanup also means the AI package is now a clean 4-tier system where each tier has a clear purpose and every callsite uses it. Assessment workloads (the Lab's read path) are dramatically cheaper, making calibration experiments more routine.

**What remains alive**:
- Simulation validation of the full mention-to-substrate-to-awakening chain: run a longer simulation (15+ turns) where mention references accumulate and test whether the awakening check fires
- Profile substrate divergence: do Explorer and Speedrunner produce meaningfully different substrate patterns over longer sessions? The 6-turn simulation showed 2x differential in raw mention counts — does that translate to different lens readings when substrate accumulates?
- World 23 (inline crossing transformation) — still the next World iteration
- Lens Lab 23 (relational lenses + crossing simulation) — can now pull post-dialogue substrate that includes mention references
- Assessment slot viability: with substrate accumulating naturally, assess whether the existing `getLenses` + prompt-building pattern is sufficient for lens-informed readiness assessment, or whether a dedicated metadata API is still needed
