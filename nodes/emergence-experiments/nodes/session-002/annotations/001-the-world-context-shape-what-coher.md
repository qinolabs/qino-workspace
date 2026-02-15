---
author: agent
signal: reading
created: 2026-02-07T18:43:33Z
---
**The world context shape** — what coherence needs beyond `{{substrate}}` and `{{scenarioContext}}`:

```typescript
interface WorldContext {
  name: string;
  description: string;
  establishedTone: string;  // the world's overall character
  scenarios: Array<{
    name: string;
    description: string;
    atmosphericTone: string;
    stage: string;  // how developed this scenario is
    keyManifestations: string[];  // names, for relationship checking
    currentTension: string | null;
  }>;
  worldHistory: string[];  // passages discovered, scenarios completed, significant events
}
```

This is the **third template variable**: `{{worldContext}}`. The current pipeline has `{{substrate}}` (what's accumulated in the assessed scenario) and `{{scenarioContext}}` (metadata about the assessed scenario). World context is everything *around* the scenario — the fabric it needs to fit.

**Key constraint**: world context should be *summarized*, not exhaustive. The LLM gets 500 max tokens for output. If world context is too large, the prompt overwhelms the assessment. The world context is about tone, themes, and relationships — not full substrate dumps of every scenario.
