# Lens Capture Command Seed

**Captured:** 2026-01-15

> The best lenses are discovered through practice. This note captures the idea for a `/qino lens-capture` command and includes the first lens discovered this way.

---

## The Idea

In conversation, lenses emerge. You don't plan them — you notice them. A distinction crystallizes, and suddenly you see: *this is a way of listening*.

The moment of discovery is fragile. If not captured, it dissipates.

**Proposed command:** `/qino lens-capture` or `/qino:lens capture`

**What it would do:**
1. Pause the current thread
2. Ask: "What distinction just emerged?"
3. Capture the lens in multiple formats (we don't know the best format yet)
4. Store as a lens seed in concepts-repo
5. Return to the thread

**When to invoke:**
- "These are lenses at work"
- "This distinction could be a way of listening"
- "I notice a sensitivity forming"

---

## First Lens Captured: Mirror-Mode / Echo-Mode

Discovered during design philosophy review. The distinction between how the system crosses when returning something to the user.

### Capture Format A: Table (Relational Contrasts)

| Dimension | Mirror-mode | Echo-mode |
|-----------|-------------|-----------|
| **Intent** | Preserve the gesture | Transform the gesture |
| **Processing** | Minimize interpretation | Allow interpretation to enter |
| **What crosses** | The shape, as intact as possible | The shape, with new edges revealed |
| **System posture** | Receiving | Participating |
| **Invites** | Recognition — "yes, that's what I said" | Discovery — "oh, I hadn't seen that" |

### Capture Format B: Narrative (Phenomenological)

**Mirror-mode** is when the system tries to be transparent. You speak, it returns your words with minimal addition. The gift is seeing yourself clearly — recognition. The system's posture is receiving: holding steady, not adding its own resonance. You meet yourself.

**Echo-mode** is when the system participates. You speak, it returns something slightly transformed. The "error" — the gap between what you said and what returns — is where discovery happens. The system's posture is participating: adding resonance, letting interpretation in. You meet something new.

### Capture Format C: Minimal (Seed Form)

```
lens: mirror-echo
polarity: preserve ↔ transform
question: is the system receiving or participating?
gift-mirror: recognition (you meet yourself)
gift-echo: discovery (you meet something new)
```

### Capture Format D: Operational (For Agent)

**When to use mirror-mode:**
- User needs to see their own words clearly
- Reflection without addition serves
- "Did I understand correctly?" moments
- Checking alignment

**When to use echo-mode:**
- User is exploring, not confirming
- Something new might be ready to emerge
- "What else is here?" moments
- Generative dialogue

---

## Notes on Format

**What worked about Format A (Table):**
- Side-by-side comparison makes the distinction sharp
- Multiple dimensions reveal the lens isn't one-dimensional
- Easy to scan

**What worked about Format B (Narrative):**
- Captures the phenomenology — what it *feels like*
- Explains the gift of each mode
- More accessible to someone new to the distinction

**What worked about Format C (Minimal):**
- Seed that can grow
- Compact reference
- Named polarity

**What worked about Format D (Operational):**
- Actionable — agent knows when to use which
- Grounded in moments, not abstractions

---

## Open Questions

- Where do lens seeds live? `ecosystem/lens/seeds/`? `notes/`?
- What's the relationship between lens seeds and full lens specifications?
- How does a seed become a lens? What's the maturation process?
- Should capture include the conversation context where the lens emerged?
