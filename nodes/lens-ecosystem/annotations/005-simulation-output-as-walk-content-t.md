---
author: agent
signal: proposal
target: content/terrain.md#VII
created: 2026-02-15T18:08:29.132Z
---
## Simulation Output as Walk Content — The Recursive Loop

### The Insight

The simulation pipeline produces rich narrative content (character dialogue, atmospheric prose, mention emergence patterns). qino-walk has a complete voice delivery pipeline (LLM text → TTS → R2 cache → audio playback). Connect them: **listen to simulation results while walking, observe what works and what doesn't, feed observations back to calibrate content and system.**

This closes the ecological loop: simulate → listen → observe → calibrate → simulate again. The human becomes part of the system's feedback mechanism, but through embodied experience (walking) rather than desk-bound analysis.

### Which Walk Mode

**Story-Walk** is the natural fit:
- Already narrates character dialogue with speaker attribution
- Per-POI content delivery maps to per-turn simulation content
- Decision points could map to "what would you ask next?" moments
- Witnessed moments (manifestation interiority panels) could show simulation metadata

**Topic-Walk** as alternative:
- "I want to think about how threshold performs for speedrunners"
- Narrates findings as vocabulary, companion comments reflect on implications
- Better for assessment/analytical listening rather than narrative immersion

### Technical Integration Points

1. **Simulation → Walk content adapter**: Transform `SimulationTurn[]` into Walk fragments (text + speaker + area context)
2. **TTS generation**: Reuse Walk's existing `TTSService` — different voices per character possible
3. **Session creation**: New walk session type or story-walk variant that sources from simulation results rather than arc narrative
4. **Observation capture**: Walk already has "witnessed moments" — extend with a capture mechanism for walker observations that flow back to simulation annotations

### Relationship to Existing Plan

- Phase 3.2 plans "Walk Backend: Atmosphere Simulation" (Walk *generating* simulated content)
- This proposal is the inverse: Walk *consuming* simulation results as listenable content
- Together they form a bidirectional loop between simulation and Walk modality
- This is "modalities as outer lenses" (terrain VII) applied reflexively — Walk becomes the perception slot through which the human experiences simulation output

### What This Dog-Foods

- Dialogue quality (do characters sound right when spoken aloud?)
- Mention density (do references feel natural in audio flow or jarring?)
- Scenario pacing (does the turn structure create good narrative rhythm?)
- Deck design (which worlds are most compelling to walk through?)
- Profile differentiation (can you hear the difference between Explorer and Speedrunner sessions?)

### When to Build

After Phase 2 stabilizes and we have enough simulation data to make the listening experience substantive. Could be a Walk iteration (16+) or a simulation Phase 3 extension. The proposal annotation on Walk's implementation node should cross-reference this one.
