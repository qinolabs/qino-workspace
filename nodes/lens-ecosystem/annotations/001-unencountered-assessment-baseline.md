---
signal: tension
date: 2026-02-15
status: open
---

# Unencountered Assessment Baseline — Full 13 Decks

All 13 starter decks assessed through 4 readiness lenses at unencountered state. This baseline tests the **assessment system itself** — do the lenses produce meaningful, differentiating signal?

## Signal Landscape

| Deck | Mfst | Cross | Conv | Sat | Fert | Clos | Anom |
|------|:----:|:-----:|:----:|:---:|:----:|:----:|:----:|
| Threshold | 3 | 3 | 0.7 | 0.3 | 0.8 | 0.1 | |
| Caravanserai | 6 | 4 | 0.75 | 0.75 | 0.85 | 0.15 | |
| Observatory | 6 | 5 | 0.7 | 0.65 | 0.85 | 0.3 | |
| Nomad Fleet | 6 | 5 | 0.85 | 0.7 | 0.85 | 0.3 | |
| Salvage Yard | 6 | 4 | 0.7 | 0.7 | 0.85 | 0.15 | |
| Comet Riders | 6 | 4 | 0.75 | 0.75 | 0.85 | 0.3 | |
| Tide Pool | 8 | 4 | 0.75 | 0.75 | 0.85 | 0.3 | |
| Abandoned Lot | 6 | 6 | 0.75 | 0.75 | (fail) | 0.15 | |
| Night Train | 5 | 6 | 0.75 | 0.75 | 0.85 | 0.3 | |
| Floating Market | 6 | 5 | 0.75 | 0.75 | 0.85 | 0.2 | |
| Coral Necropolis | 6 | 5 | 0.75 | 0.75 | 0.85 | 0.3 | |
| Metro Depths | 8 | 5 | 0.75 | 0.75 | 0.85 | 0.3 | |
| Troll Wood | 6 | 0 | 0.85 | 0.75 | 0.85 | 0.75 | Y |

## System-Level Findings

### 1. The lenses are not differentiating at unencountered state

| Lens | Mean | Stdev | Range | Diagnosis |
|------|:----:|:-----:|:-----:|-----------|
| fertility | 0.85 | 0.01 | [0.8, 0.85] | Constant — cannot differentiate |
| convergence | 0.75 | 0.05 | [0.7, 0.85] | Very tight clustering |
| saturation | 0.70 | 0.12 | [0.3, 0.75] | Best spread, but single outlier (Threshold) |
| closure | 0.28 | 0.16 | [0.1, 0.75] | Spread driven by single outlier (Troll Wood) |

This is a system finding, not a deck finding. The lenses read 13 different decks nearly identically. Two possible interpretations:
- **Benign**: Unencountered state genuinely doesn't differentiate — all well-designed decks look similarly fertile and unsaturated before interaction. The lenses will differentiate when user substrate exists (Phase 2).
- **Concerning**: The assessment prompts may be too generic, producing "safe" middle-range signals regardless of input. The lenses need sharper discrimination criteria.

### 2. Anomaly detection is too conservative

Only 1 of 13 decks flagged. When fertility is constant (stdev 0.01), the detector should notice that the lens itself is undifferentiated — the diagnostic unit is the lens, not the deck.

### 3. JSON parsing fails on long narratives

Abandoned Lot's fertility assessment produced valid signal (0.85) but the LLM response was truncated mid-sentence, breaking JSON parsing in `assessment-service.ts:142`. The regex `/\{[\s\S]*\}/.exec()` can't recover from incomplete JSON. This will recur for decks with rich substrate that produces verbose narratives.

### 4. Troll Wood remains the sole anomaly

Closure 0.75, convergence 0.85, 0 crossings. Whether this reflects the folkloric genre or an actual assessment calibration issue is testable — design a minimal deck with "pre-established rules" and see if closure is also high.

## Implications for Phase 2

The unencountered baseline establishes that the system needs **user substrate** to produce differentiating signal. The key hypothesis for Phase 2: when simulated user profiles (curious-explorer, lore-seeker, relationship-builder, speedrunner) produce different substrate, do the lens profiles diverge? If they don't, the lenses themselves need recalibration.

## Bugs Found

- `assessment-service.ts:142` — JSON parse failure on truncated LLM narratives. Needs truncation recovery or max_tokens increase.

## Raw Data

52 assessment facets in local D1 (`user_id = 'simulation'`). All 13 decks × scenario 0 × 4 lenses (minus 1 failure = 51 successful + 1 error).
