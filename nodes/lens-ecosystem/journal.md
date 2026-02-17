

<!-- context: session/2026-02-15 -->

## Simulation Surface: First Real Run

Committed Phase 2 implementation and ran first simulation against threshold deck.

**What happened**: Full 4-profile comparison against frontier-station scenario 0 (6 turns each, ~72 LLM calls). Pipeline works end-to-end: Lens Lab orchestrates, World runs dialogue with real pipeline, mentions extracted from every response.

**Finding**: Mention extraction scales linearly with engagement depth (Explorer 8, Lore Seeker 7, Relationship Builder 5, Speedrunner 4). Lens signals are identical across profiles because v1 doesn't accumulate mention substrate — the differential signal lives in raw mention counts.

**Bug fixed**: Signal averages in profile comparison were using `Math.round()` which collapsed 0.7 → 1 and 0.3 → 0. Fixed to preserve 2 decimal places.

**What remains**: Model research running in background (reviewing OpenRouter options for simulation cost optimization). Findings annotation written to lens-ecosystem navigator.
