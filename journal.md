


## captured: Lens Ecosystem Integration

<!-- context: node/lens-ecosystem -->

captured: Lens Ecosystem Integration
→ [lens-ecosystem](nodes/lens-ecosystem/)


<!-- context: session/2026-02-09 -->

**Lens Ecosystem navigator created.**

Maps how lens technology crosses from Journey infrastructure into living modality experience. Territory spans Journey voicing (infra 09), World integration (23 iterations, crossing slot live), Walk integration (concept-rich, indirect consumption), Lens Lab (calibration surface), and the concept architecture (24 lenses, 8 slots, dialect system).

Sparked by today's validation session — building ecosystem calibration revealed the nervous system boundary. `voiceForSlot` conflates ecosystem-level and modality-local operations. The navigator carries this discovery forward as a terrain map with 12 open questions, reading order, and three convergent tracks toward the missing piece: `getLensContext`.

Sibling to emergence-experiments — that one maps what emerges, this one maps how sensitivity reaches the organs.


<!-- context: session/2026-02-15 -->

**Simulation prototyping surface live — first experiment run.**

Phase 1 built and deployed: starter deck adapter + simulation router in lens-lab-backend. 5 of 13 decks assessed through all 4 readiness lenses at unencountered state. Key finding: Troll Wood triggers the anomaly detector — closure at 0.75 (vs 0.1–0.3 for other decks) at unencountered state, 0 crossings, all signals undifferentiated. Fertility stable across all decks (0.8–0.85), suggesting it measures design potential rather than differentiating between decks at this stage. Saturation correlates with manifestation count but not linearly — Night Train (5 figures) matches Tide Pool (8 figures).

Simulation-operator agent definition created. Findings written to → [lens-ecosystem annotations](nodes/lens-ecosystem/annotations/001-unencountered-assessment-baseline.md). 20 assessment facets persisted in D1. 8 decks remain unassessed. Phase 2 (user behavior simulation) is where real differentiation should emerge.


<!-- context: session/2026-02-15 -->

**Claude Code session history: known bug, no fix.**

The `/resume` picker only shows ~10 most recent sessions (hardcoded batch size). On this workspace, 668 session files exist spanning Dec 2025 to Feb 2026 — all intact on disk, but invisible in the picker.

Root cause: the picker scans `.jsonl` files in `~/.claude/projects/` sorted by mtime, returns only the first 10, and the load-more pagination is broken. Separately, `sessions-index.json` stopped updating on Feb 3 during an API outage, but v2.1.42 doesn't read from it anyway.

10+ open issues on anthropics/claude-code (#22205, #24435, #24729, #25032, #25685 and others), earliest from Jan 31. Zero official responses from Anthropic. Compiled Mach-O binary on macOS ARM — community `sed` patch for `cli.js` doesn't apply.

Workaround: `claude --resume <session-id>`. Session IDs findable via `~/.claude/history.jsonl`.


## created: Portfolio 2024 — Static Rebuild

<!-- context: node/portfolio-2024 -->

created: Portfolio 2024 — Static Rebuild
→ [portfolio-2024](nodes/portfolio-2024/)


## created: Strapi Content Model Analysis

<!-- context: node/portfolio-strapi-analysis -->

created: Strapi Content Model Analysis
→ [portfolio-strapi-analysis](nodes/portfolio-strapi-analysis/)


## created: Gatsby Frontend Architecture Analysis

<!-- context: node/portfolio-gatsby-analysis -->

created: Gatsby Frontend Architecture Analysis
→ [portfolio-gatsby-analysis](nodes/portfolio-gatsby-analysis/)


## created: Static App Migration Strategy

<!-- context: node/portfolio-migration-strategy -->

created: Static App Migration Strategy
→ [portfolio-migration-strategy](nodes/portfolio-migration-strategy/)


<!-- context: session/2026-02-16 -->

**Portfolio 2024 navigator created — static rebuild of Gatsby + Strapi portfolio.**

Cloned both original repos (`portfolio-colocate-strapi`, `portfolio-gatsby`) into workspace. Deep-dived both architectures in parallel:

- **Strapi**: 4 collection types, 1 single type, 7 shared components, zero custom logic. All controllers/services/routes are default CRUD. PostgreSQL (prod) + S3 media. Key finding: the CMS is pure plumbing with no business logic — entirely replaceable.
- **Gatsby**: 4 pages, ~30 components, D3 Voronoi visualization (most complex piece), Tailwind 3 + SCSS dark mode, Jotai state with URL hash sync, custom GraphQL field extensions for undraw SVGs and area color mixing.

**Bold architectural decision**: No CMS, no backend. Content becomes TypeScript data modules, images move to R2, app deploys as a single static TanStack Start frontend using `template-frontend-static`. Monthly cost: $0.

Created implementation directory at `qinolabs-repo/implementations/portfolio-2024/` with three deep-dive indexes:
→ [01-strapi-architecture-index](../qinolabs-repo/implementations/portfolio-2024/content/01-strapi-architecture-index.md)
→ [02-gatsby-architecture-index](../qinolabs-repo/implementations/portfolio-2024/content/02-gatsby-architecture-index.md)
→ [03-cloudflare-migration-strategy](../qinolabs-repo/implementations/portfolio-2024/content/03-cloudflare-migration-strategy.md)

Graph nodes: `portfolio-2024` (navigator), `portfolio-strapi-analysis`, `portfolio-gatsby-analysis`, `portfolio-migration-strategy` (findings).
