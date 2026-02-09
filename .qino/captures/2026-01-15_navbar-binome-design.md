# Navbar Binome Design

**Captured:** 2026-01-15

> Nav items as pairs/polarities rather than single terms — holding relational tension.

---

## The Idea

What if nav items aren't single terms like "Language" or "Apps"?

What if they're **pairs** — binomes, polarities — that hold relational tension, or facets of something that can't be found in a single term?

### Examples

| Binome | Tension Held |
|--------|--------------|
| Apps≈Modalities | Product ↔ Pattern |
| Language≈Poetry | Structure ↔ Expression |
| Research≈Tools | Inquiry ↔ Instrument |
| Journal≈Chronicle | Personal ↔ Narrative |

---

## Visual Design

Satisfying larger rectangular padded tiles with hover effect:

```
┌─────────────────────────┐
│  ┌────┐                 │
│  │icon│  Term A         │
│  │    │  Term B         │
│  └────┘                 │
└─────────────────────────┘
```

**Left side:** Square icon, abstract texture, characteristic color palette

**Right side:** Two terms vertically stacked, differentiated in color

---

## Dynamic Behavior

The order of terms changes based on orientation:

| State | Term Order | Why |
|-------|------------|-----|
| Approached-from | Structure term on top | You're entering, seeking form |
| Looked-from (page open) | Process term on top | You're inside, seeing flow |

**Example:**
- Navigating TO Apps≈Modalities → shows "Apps" on top (structure, product)
- INSIDE Apps≈Modalities → shows "Modalities" on top (process, pattern)

---

## Connection to Structure↔Process Polarity

This embodies the insight from design-philosophy:
- Structure and process aren't opposed — they're facets
- The relationship shifts based on where you stand
- Both terms are always present; what changes is which is foregrounded

The nav item doesn't resolve the tension — it **holds** it.

---

## Open Questions

- What icon/texture language works for abstract binomes?
- How does hover transition feel? Smooth term-swap or just color shift?
- Does the ≈ symbol appear in the UI or is it implicit in the stacking?
- How does this scale to mobile nav?
