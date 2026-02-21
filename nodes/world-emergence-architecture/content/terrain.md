# World Emergence Architecture — Terrain

## Territory

The substrate system evolving from session-local observation into a world-level emergence engine. Everything between "mentions get detected" and "the world feels alive because your attention shaped it."

---

## Reading order

### I. The experience we're building toward

1. **emergence-payoff-loop** → `qino-concepts/concepts/emergence-payoff-loop/story.md`
   The loop: attention → accumulation → threshold → emergence → payoff → new attention.

2. **emergence-payoff-loop examples map** → `qino-concepts/concepts/emergence-payoff-loop/content/examples-map.md`
   Six concrete examples at increasing scale. Manifestation awakening, area awakening, walk→world crossing, world awakening, relational inquiry, temporal passage.

### II. What enables the experience

3. **generative-context-design** → `qino-concepts/concepts/generative-context-design/story.md`
   The author's instrument. Seeds, constraints, context composition.

4. **generative-context-design initial map** → `qino-concepts/concepts/generative-context-design/content/initial-map.md`
   Three layers (user input, context seeds, prompt shaping), six seed types, worked example of how a first mention emerges, the bidirectional loop, the author's toolkit.

### III. The awakening design

5. **proposal 003** → `qinolabs-repo/implementations/qino-world/annotations/003-awakening-cinematic-overlay.md`
   Updated: ceremony as in-world deepening, metabolized substrate as portrait, inline notification, origin varies (ancient/new/liminal), type-general (manifestation/area/world).

6. **proposal 005** → `qinolabs-repo/implementations/qino-world/annotations/005-scenario-knowledge.md`
   Deferred concept — how scenario knowledge feeds context composition.

### IV. What's built

7. **substrate system** (code audit)
   - Schema: `qinolabs-repo/apps/qino-world-backend/src/db/schema.ts` (lines 219-305)
   - Detection: `qinolabs-repo/apps/qino-world-backend/src/routes/chat-stream.ts` (lines 448-851)
   - Extraction: `qinolabs-repo/apps/qino-world-backend/src/services/mention-extraction.ts`
   - Accumulation: `qinolabs-repo/apps/qino-world-backend/src/services/substrate-service.ts`
   - Awakening: `qinolabs-repo/apps/qino-world-backend/src/services/awakening-service.ts`
   - Awakening router: `qinolabs-repo/apps/qino-world-backend/src/routers/awakening-router.ts`
   - Frontend mentions: `qinolabs-repo/apps/qino-world/src/features/chat/mention-highlight.tsx`

### V. The concept foundation

8. **qino-world concept** → `qino-concepts/concepts/qino-world/content/concept.md`
   Sections: "Mentions Awaken Through Curiosity", "Passages", "Memory", "The Encounter Space"

9. **crossing examples** → `qino-concepts/concepts/crossing-examples/content/crossing-examples.md`
   Station-keeper crossing worlds, nutrient cycle → walk, walk capture → world conversation.

---

## The three structural gaps

### Gap 1: Substrate scope

```
    CURRENT                          NEEDED
    ───────                          ──────

    mentionSubstrate keyed on        substrate that accumulates
    (sessionId, mentionName,         across sessions within a
     mentionType)                    scenario or world

    if you mention Rix in            when you mention Rix in
    session A and deepen in          session A and deepen in
    session B → two separate         session B → same substrate,
    records                          richer

    session ends → substrate         substrate persists as part
    is orphaned                      of the world's memory
```

**Questions:**
- What's the right scope — scenario or world? A mention in one scenario might matter in another (especially for world-level emergence).
- How do we preserve the contextual richness of where each perspective came from while aggregating across sessions?
- Does the `relationalInquiries` table need to change, or just the parent `mentionSubstrate`?
- Migration path: can we lift existing session-scoped substrate into scenario scope without losing data?

### Gap 2: The feedback loop (latent seeds → context → LLM)

```
    CURRENT (one-way)

    LLM output ──▶ detection ──▶ storage
                                    │
                                    └── (dead end — not fed back)


    NEEDED (circular)

    storage ──▶ context composition ──▶ LLM input
       ▲                                    │
       │                                    ▼
       └──── detection ◀──── LLM output
```

**Questions:**
- Where does latent seed injection happen in the prompt composition pipeline? Currently, `dialogue-service.ts` composes the prompt. It already has `phaseContext` (temporal seed). What other seeds need to be injected?
- How do we select which latent seeds to include? A manifestation's knowledge boundaries should determine which latent areas/manifestations they can reference. But the starter deck content doesn't currently carry this information.
- Context budget: how much space do seeds take? The prompt already includes world context, manifestation identity, conversation history. Adding latent seeds, relational seeds, and substrate summaries competes for tokens.
- The substrate-to-seed pipeline: how does accumulated player attention (what they've asked about, what they've lingered on) become future context? This is the responsiveness mechanism.

### Gap 3: Emergence types beyond manifestation

```
    BUILT                            NOT BUILT
    ─────                            ─────────

    mention types:                   area substrate:
    person, place, artifact,         accumulation at area level,
    faction, planet                  threshold for area awakening

    manifestation awakening:         world substrate:
    creates new manifestation        cross-scenario accumulation,
    with scope "world"               threshold for new world/passage

                                     relational threading:
                                     manifestation needs/requests
                                     that connect through the web
                                     of relationships
```

**Questions:**
- Does area awakening use the same substrate pipeline (mention detection → accumulation → threshold) or a different mechanism?
- How does the system distinguish between "this place was mentioned" and "this place is accumulating enough depth to become accessible"? Is it just perspective count, or something richer?
- World awakening is the rarest emergence. What's the threshold? Cross-session, cross-manifestation — how much substrate is "enough"?
- Relational threading (manifestation requests/needs) — is this substrate-driven or does it need its own system? Can a manifestation's "unresolved need" be a kind of latent seed?

---

## Open questions (cross-cutting)

1. **The relational inquiry gesture** — the drag-onto-mention interaction isn't built. This is the smallest unit of the emergence loop and the most frequent payoff. How urgent is it vs. the structural gaps?

2. **Portrait generation** — the ceremony needs a metabolized description, not raw substrate. This is an AI generation design problem: synthesize fragments into coherent in-world text that fills gaps and creates curiosity. Different from first-speech generation (which already exists).

3. **The anti-pattern** — when does emergence feel hollow? When does "shaped probability" collapse into "scripted response"? We need to recognize this early.

4. **Starter deck as first seed authoring exercise** — the existing starter decks are the first latent seed pool. Do they carry enough depth (latent areas, latent manifestations, relational maps) to support first mentions? Or do they need enrichment?

5. **Simulation validation** — the simulation system (iteration 26) can validate emergence patterns. Can we simulate the full loop: mention → accumulation → awakening → payoff?

6. **Pacing** — how often should each type of emergence happen? The examples map shows a scale from frequent (relational inquiry) to rare (world awakening). But what's the right cadence in practice?

---

## Convergent tracks

Three paths through this territory that could each become an iteration:

### Track A: Substrate evolution ✓ (iteration 28)
Substrate lifted from session-scope to scenario-scope. `mentionSubstrate` keyed on `scenarioId` — accumulates across all sessions in a scenario. Feedback loop partially closed: worldSeeds (latent manifestations, areas, relational maps) threaded through simulation and dialogue pipelines. Substrate-to-seed responsiveness (accumulated attention shaping future context) deferred to emergence types work.

### Track B: Awakening ceremony ✓ (iteration 27)
Cinematic ceremony rewrite using crossing components (CrossfadeBackdrop, TypewriterText, TextBackground). Portrait generation service metabolizes substrate into coherent in-world description with origin classification (ancient/new-arrival/liminal) and area tracing. Inline notification in chat ("Something stirs...") when substrate crosses threshold. Proposal 003 resolved.

### Track C: Seed authoring ✓ (iteration 27)
Starter deck schema extended with latent manifestations, latent areas, and relational maps with visibility tiers (open/hidden/latent). Existing decks enriched with 2-3 latent manifestations, 1-2 latent areas, and relational map entries per deck.

### Track D: World autonomy (emerged from world-inner-life concept)
The world processes substrate autonomously — manifestations metabolize accumulated attention independently of user sessions. Three expressions: ambient chatter (manifestation rail as living periphery), manifestation emotional state (atmospheric shifts from world processing), pocket participation (mobile async choices). Substrate becomes the world's nervous system. Relational map visibility tiers govern what the autonomous process can surface. Depends on Track A (scenario-scoped substrate).

**Dependencies:**
- ~~Track B can proceed on current substrate~~ → complete
- ~~Track A must precede area/world awakening AND world autonomy~~ → complete, Track D unblocked
- ~~Track C can proceed in parallel~~ → complete, visibility tiers now also constrain Track D
- Track D depends on Track A (world process needs persistent substrate to metabolize)

---

## Session log

### 2026-02-21 — Navigator created

**Territory mapped from today's session:**
- Wrapped proposal 004 (scenario menu) — committed, annotation resolved
- Deep design session on proposal 003 (awakening ceremony): ceremony as in-world deepening, portrait metabolized from substrate, origin varies, inline notification, not first speech
- Created emergence-payoff-loop concept node with six concrete examples at increasing scale
- Created generative-context-design concept node with three-layer model, seed taxonomy, bidirectional loop
- Substrate system audit: detection pipeline is solid, accumulation works, awakening mutation works. Three structural gaps identified (scope, feedback loop, emergence types)
- Key design insight: unlocking is bidirectional — the world opens through your understanding, and your understanding deepens through what the world opens. Manifestation requests/needs as relational threads, not quests
- Key system insight: the LLM's coherence-seeking IS the emergence engine, if context is designed well. The author's craft is in seed placement and constraint curation

### 2026-02-21 (continued) — Iteration 27 complete + world inner life

**Iteration 27 shipped (Awakening Ceremony & Seed Foundation):**
- Phase 1: Seed foundation — starter deck schema extended with latent manifestations, latent areas, relational maps (visibility tiers: open/hidden/latent). Existing decks enriched.
- Phase 2: Portrait generation service — substrate → coherent portrait via LLM. Origin classification (ancient/new-arrival/liminal). Area tracing through messageMentions → messages → areas join.
- Phase 3: Awakening ceremony UX — cinematic rewrite reusing crossing components (CrossfadeBackdrop, TypewriterText, TextBackground). Three-phase ceremony: recognition → portrait reveal → invitation.
- Phase 4: Inline notification — query-based reactivity (not SSE piggybacking). When stream completes, invalidate substrate query, detect new awakenable substrates. "Something stirs... {mentionName}" notification with auto-dismiss.
- Proposal 003 (awakening cinematic overlay) → resolved

**World inner life concept captured (qino-concepts ecosystem node):**
- Design vision: remove drag gesture from chat, bring manifestation rail to life with ambient chatter, manifestations metabolize substrate autonomously
- Three expressions: ambient chatter, manifestation emotional state, pocket participation (mobile)
- Fourth layer of generative context design: what the world generates for itself
- Substrate as the world's nervous system — user interactions → substrate → world processing → more substrate
- Extends emergence-payoff-loop and generative-context-design concepts
- Track D (world autonomy) emerged as new convergent track

**Thread connections made:**
- Annotated world-emergence-architecture with world-inner-life connection (extends Gap 2)
- Annotated dialogue-quality proposal 002 (natural mention detection) with drag gesture removal direction — communication over mechanical action strengthens LLM-native detection approach
- Proposal 005 (scenario knowledge) gains relevance: manifestation hints become one expression of world inner life

### 2026-02-22 — Track A complete (iteration 28)

**Substrate scope lift shipped:**
- `mentionSubstrate` now keyed on `scenarioId` — cross-session accumulation within a scenario works
- Migration simplified to `ALTER TABLE ADD COLUMN` due to D1 PRAGMA limitation (D1 doesn't persist `PRAGMA foreign_keys=OFF` across statement breakpoints)
- `sessionId` stays NOT NULL in DB (cosmetic mismatch with Drizzle schema) — threaded through all insert paths
- Substrate service, chat-stream, simulation pipeline, substrate router, frontend queries all rekeyed to scenarioId

**Track D unblocked:** With substrate persisting at scenario level, world autonomy (ambient chatter, manifestation emotional state) has the foundation it needs. The world's nervous system is now scenario-scoped.

**What remains in this territory:**
- Gap 2 (feedback loop) partially closed — worldSeeds flow into dialogue, but accumulated attention doesn't yet shape future seeds
- Gap 3 (emergence types) — area and world awakening need their own accumulation logic on top of the now-scenario-scoped substrate
- Track D (world autonomy) — unblocked but not started
