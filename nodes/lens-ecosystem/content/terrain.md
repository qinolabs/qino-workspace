# Lens Ecosystem Integration — Terrain

## I. The Nervous System Discovery (What This Session Proved)

The architectural boundary that shapes everything in this territory. Understanding this is prerequisite.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Ecosystem calibration concept** | `qino-concepts/concepts/tech-qino-lens/content/ecosystem-calibration.md` | The nervous system metaphor: Journey carries sensitivity, organs generate experience. Validated by building. Contains the vocabulary-to-grammar framing. |
| **Validation learnings** | Same file, "Validation Learnings" section (2026-02-09) | The two builds that revealed the boundary: Walk forced its data into `userNote`, proving `voiceForSlot` conflates two operations. Walk wiring was removed. |
| **Infra 09 post-validation status** | `implementations/qino-infra/content/09-ecosystem-calibration.md`, "Post-Validation Status" section | What was kept (adaptation tables, `mergePromptWithAdaptation`, `voiceForSlot` for ecosystem ops), what was simplified (Walk profile/adaptations removed), all 8 RPC methods preserved. |
| **Design principles** | `qino-concepts/concepts/ecosystem-design-principles/content/design-principles.md` | "Trust the Ecosystem" and "Boundaries of Meaning" — the two principles this boundary instantiates. Journey provides intelligence; modalities own their experience. |

**The pattern**: `voiceForSlot` is correct for crossing and assessment (ecosystem-level). For narrator, companion, soundscape, inhabiting (modality-local), the correct flow is: modality asks Journey for lens data, Journey returns metadata, modality generates content with its own prompts enriched by that metadata.

---

## II. Journey Infrastructure (What Exists)

The nervous system's current wiring — what's built, what's proven, what's ready for extension.

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Voicing service** | `qino-infra/journey/src/services/voicing-service.ts` | Core pipeline: `selectLens` (7-step cascade), `mergePromptWithAdaptation`, `voiceForSlot`, substrate fingerprinting. The enhanced `selectLens` considers figure composite, slot defaults, layer preferences. |
| **Modality profile service** | `qino-infra/journey/src/services/modality-profile-service.ts` | CRUD for profiles and attention adaptations. Seeds World profile (crossing slot active). Walk profile removed post-validation. |
| **Journey schema** | `qino-infra/journey/src/db/schema.ts` | Tables: `lenses` (24 seeded), `facets` (with `slotType` column), `modality_profiles`, `attention_adaptations`, `encounters`, `substrates`, `arcs`. The `facets.slotType` column is the slot-aware cache dimension. |
| **Journey RPC interface** | `qino-infra/journey/rpc.ts` | 8 iteration-09 methods: `registerModalityProfile`, `getModalityProfile`, `listModalityProfiles`, `setAttentionAdaptation`, `getAttentionAdaptations`, `voiceForSlot`, `seedModalityData`. Plus existing voicing and lens methods. |
| **Slot type vocabulary** | `qino-infra/journey/src/types.ts` | 8 `SlotType` values: crossing, theme, inhabiting, narrator, companion, soundscape, assessment, current. `SLOT_DEFINITIONS` constant with descriptions. Only `crossing` is currently exercised in production. |

**What's proven**: Crossing voicing through `voiceForSlot` with World's modality profile works. Attention adaptation merging works. Slot-aware caching works. The 7-step lens selection cascade handles null profiles gracefully.

**What's missing**: A `getLensContext` method that returns lens selection + adaptation metadata WITHOUT generating content. This is the API that enables modality-local integration.

---

## III. World Integration (The Working Consumer + Next Steps)

World is 23 iterations deep. It has the richest infrastructure for lens integration and the one proven integration point (crossing).

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **World voicing service** | `apps/qino-world-backend/src/services/voicing-service.ts` | Local facet generation for crossing. Builds prompts from manifestation + world + scenario context. |
| **Transformation service** | `apps/qino-world-backend/src/services/transformation-service.ts` | Unified AI call: `worldName + facet + areaId` during crossing ceremony. The crossing integration that `voiceForSlot` feeds into. |
| **Crossing composition** | `apps/qino-world-backend/src/services/crossing-composition-service.ts` | Orchestrates crossing: resolution via Weave, voicing via Journey, placement via areas. The full pipeline from ecosystem figure to World manifestation. |
| **Awakening service** | `apps/qino-world-backend/src/services/awakening-service.ts` | Mention-to-manifestation pipeline. Future site for readiness lens integration (assessment slot). |
| **World prompt builder** | `apps/qino-world-backend/src/services/world-prompt-builder.ts` | Builds dialogue context from area + nearby figures. Future site for narrator slot integration — figures present should shape narrative voice. |
| **Iteration 23 (inline crossing)** | `implementations/qino-world/content/23-inline-crossing-transformation.md` | Next World iteration. Wires transformation into inline crossing path. Uses existing `voiceForSlot` correctly. |

**What World needs from the lens intelligence API** (post-Track-3):
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

**What Walk needs from the lens intelligence API**:
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
1. Lab reads ecosystem configuration (which profiles, which adaptations, which slots active)
2. Lab measures lens behavior against real and fixture substrate
3. Lab surfaces anomalies and cross-grammar discoveries
4. (Future) Lab writes calibration-informed adjustments back to Journey via `setAttentionAdaptation`

The read path is built. The write path (Lab adjusting Journey's adaptations) is the eventual close of the feedback loop.

---

## VI. Concept Architecture (The Design Language)

| What | Where | Why It Matters Here |
|------|-------|---------------------|
| **Core lens concept** | `qino-concepts/concepts/tech-qino-lens/content/concept.md` | Bidirectional gaze, scope vs slot, emergence layer, anti-fixation. The foundational document. |
| **Lens slots and pipelines** | `qino-concepts/concepts/tech-qino-lens/content/lens-slots-and-pipelines.md` | Complete slot taxonomy, pipeline composition. "Lenses don't scale by adding more lenses. They scale by recognizing more slots." |
| **Lenses, manifestations, figures** | `qino-concepts/concepts/tech-qino-lens/content/lenses-manifestations-figures.md` | Recursive architecture: manifestations as composite lenses. Three flows: lens voices manifestation, manifestation embodies lens, manifestation voices other substrate. |
| **Ecosystem calibration** | `qino-concepts/concepts/tech-qino-lens/content/ecosystem-calibration.md` | Nervous system metaphor, vocabulary-to-grammar, dialect vs replacement, calibration ecology. Updated with validation learnings. |

---

## Reading Order

For entering this territory fresh:

1. **The nervous system metaphor** -> `qino-concepts/concepts/tech-qino-lens/content/ecosystem-calibration.md` (first 3 sections) — understand vocabulary vs grammar, the dialect principle, sensitivity allocation
2. **The validation discovery** -> same file, "Validation Learnings" section — understand why Journey generates for ecosystem ops and modalities generate for local ops
3. **The core lens concept** -> `qino-concepts/concepts/tech-qino-lens/content/concept.md` (sections 1-2) — bidirectional gaze, scope vs slot
4. **What's built** -> `implementations/qino-infra/content/09-ecosystem-calibration.md` — the iteration that built the infrastructure, read goals + post-validation status
5. **The slot taxonomy** -> `qino-concepts/concepts/tech-qino-lens/content/lens-slots-and-pipelines.md` (Slot Architecture section) — understand where lenses operate
6. **World's readiness** -> `implementations/qino-world/story.md` + `content/23-inline-crossing-transformation.md` — the working consumer and its next step
7. **Walk's constraint** -> `implementations/qino-walk/story.md` — "No figure addresses the walker" and the three-mode architecture
8. **The laboratory** -> `implementations/lens-lab/story.md` + `content/23-relational-lenses-and-crossing.md` — where calibration happens
9. **Design principles** -> `qino-concepts/concepts/ecosystem-design-principles/content/design-principles.md` — Trust the Ecosystem, Boundaries of Meaning

---

## Open Questions

### The Lens Intelligence API (`getLensContext`)

- What should the return type look like? Minimum: `{ lensId, lensName, attentionFocus, promptSuffix, facetReferences? }`. Should it include the full merged prompt or just the overlay components?
- Does it need its own caching? The lens selection result doesn't change unless the profile or adaptation changes. Could be in-memory per request.
- Should `getLensContext` live as a new RPC method, or can `voiceForSlot` be extended with a `metadataOnly: true` flag?
- Does `getLensContext` return existing facets as references? (e.g., "here's the last crossing facet for this figure" — giving the modality context about how Journey already sees the figure)

### Modality Consumption Patterns

- Which World slot activates first after the lens intelligence API exists? Narrator (shapes session voice) vs theme (shapes scenario atmosphere) vs inhabiting (shapes figure-in-scenario)?
- How does Walk's "no figure addresses the walker" principle translate into lens integration code? The lens intelligence colors the narrator, not the figure. What does the prompt structure look like?
- Do modalities need to register their slots as "active" in the modality profile before consuming `getLensContext`, or is registration separate from consumption?

### Calibration Loop + Architecture

- When does the Lens Lab calibration loop close? Lab discoveries need to feed back into Journey adaptations. The read path exists. The write path needs a Lab UI for authoring adjustments.
- Does the existing `voiceForSlot` need refactoring, or can `getLensContext` coexist alongside it?
- Should attention adaptations be versioned? (Tracking which adaptation version produced which facet, enabling rollback.)
- How do cross-grammar experiments inform the adaptation system?

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
