# Crossing as immersive threshold — voicing during visual journey

**Captured:** 2026-01-10T14:30:00Z

The crossing experience has two moments:

**1. Form Selection (quick, playful)**
- LLM suggests forms based on figure type and destination context
- User sees suggestions as tappable icons (one highlighted, others muted)
- Tap any icon to instantly switch form — no regeneration needed
- This step is fast, not blocking

**2. Immersive Crossing (the threshold)**
- When user confirms, an immersive loading screen begins
- This IS the crossing experience, not a wait
- The figure currently being voiced is shown prominently
- Background gently slides through world and area images
- Overlay shows:
  - Short world description or lore
  - Scenario context
  - For returning players: recent events, fateful decisions from previous scenarios
- Each figure gets its moment as it's voiced
- The visual journey builds anticipation and emotional context

**Key insight:** Don't hide the voicing latency — narrativize it. The 1-3 seconds per figure becomes a meaningful moment of arrival, not dead time.

**Connection to world tokens:**
- The 6 token types (👤🚀🏛️📿🌀💭) are the form vocabulary
- Form selection uses these as visual icons
- During crossing, the token icon could animate or transform as voicing completes

**What gets cached:**
- Voicing happens here, facets are cached
- Embedding happens async (waitUntil) after voicing
- Next visit to same figure = cache hit, no crossing needed
