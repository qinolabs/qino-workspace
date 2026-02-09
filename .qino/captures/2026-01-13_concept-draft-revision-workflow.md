# Concept Draft-Revision Workflow

**Captured:** 2026-01-13
**Context:** Emerged during sys-qino-cli-onboarding exploration

---

## The Problem

When heavy concept work happens, the system might get confused about:
- Whether to update revisions.md implicitly
- When to create a new revision
- Whether changes to concept.md are exploratory or settled

Risk: Implicit updates to the main concept file when work is still in flux.

---

## The Pattern

**Separation of working state and settled state.**

### When concept exploration begins:

1. Create a snapshot: `concept-draft-<timestamp>.md`
2. All organic concept work happens in the draft
3. Main concept.md remains untouched as reference
4. User can always compare working state with original

### The draft structure:

The draft could use an epistemic structure native to concept evolution:
- Working with source material
- Developing it to a new state
- Tracking what's changed and why

(Structure TBD — this is a door we don't have to enter now)

### When settling:

When the draft reaches a settling point, the system asks:
- "Create a new revision?" → Updates concept.md, logs in revisions.md
- "Save for later?" → Draft remains, concept.md untouched

### Critical rule:

**If `concept-draft-<timestamp>.md` exists, the original concept.md should NEVER be updated implicitly.**

### When new exploration starts with existing draft:

Ask the user:
- "Continue with existing draft?" → Resume work on current draft
- "Start fresh?" → Archive or discard old draft, create new one

---

## Benefits

1. **Comparison** — Always see current working state vs. original
2. **Safety** — No implicit changes to settled concepts
3. **Flexibility** — Can abandon draft without losing original
4. **Clarity** — Clear distinction between exploring and settling

---

## Doors Not Entered (Yet)

- What epistemic structure should the draft use?
- How does this relate to held_threads?
- Should drafts be git-tracked or ephemeral?
- What triggers "settling point" detection?
- How does this interact with multi-session concept work?
