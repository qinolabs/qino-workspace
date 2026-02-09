via codex

# Figure View Projection (Journey/Substrate)

**Captured:** 2026-01-10

## Concept
- Figures remain emergent: no catalog service, only encounter-backed memory.
- UI "figure lists" should be derived by deduping encounters on `figureRef`.
- Journey stays the access layer; Substrate remains the authoritative encounter store.

## Technical decisions
- Added a Journey-level "figure view" projection: latest encounter per `figureRef`.
- `figureRef` (`sourceApp:figureType:figureId`) is the stable selection key across apps.
- `listFigures` returns a presentational floor: name, type, sourceApp, sourceContext, and `lastEncounterId`.
- Removal remains per-encounter; list views use `lastEncounterId` for removal and will fall back to older encounters if present.

## Display metadata
- Image URLs are stored in `encounter.sourceContext.imageUrl` at record time.
- This keeps images tied to the encounter snapshot (not substrate state or a catalog).

## UI implications
- Cross-app "Bring Figures" uses `listFigures` and selects by `figureRef`.
- Shared Journey toggle lists figures (deduped) rather than raw encounters.
