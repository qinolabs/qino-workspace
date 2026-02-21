Maps the territory where simulation, evaluation, and prompt improvement form a closed loop — mediated by autonomous agents, visualized through qino-lab, and reviewed through qino-label.

The core insight: an agent can use the qino-protocol graph as its persistent working memory. Each evaluation node's story.md carries Hypothesis / What Changed / Results / Implications sections. The Implications section is the agent's next instruction to itself. The graph IS the state machine. The human's oversight tool is the journal and the graph visualization — not an approval workflow.

## Territory

**Three surfaces, one loop:**
- **qino-lab MCP** — agent reads/writes the investigation graph
- **qino-lab browser** — human sees the decision tree, writes journal notes
- **qino-label** — human reviews specific runs at turn-level perception

**Prompt overrides as experiment data:**
- Agents write prompt variants in `data/config.json`, not in production code
- Simulation service accepts `promptOverrides` (preamble, prohibitions, affirmations, rules, knowledgeBoundaries)
- Production defaults used when no override specified — baselines are free

**Story convention for evaluation nodes:**
- Hypothesis: what's being tested
- What Changed: diff from previous run
- Results: scores and comparison
- Implications: what to try next (the agent's decision point)

## Reading Order

1. `qinolabs-repo/implementations/evaluation-loop/story.md` — technical context and boundaries
2. `qinolabs-repo/implementations/dialogue-quality/story.md` — current prompt architecture and issues
3. `qinolabs-repo/implementations/lens-lab/content/24-simulation-framework.md` — simulation infrastructure
4. `.claude/agents/simulation-operator.md` — current agent definition (to be evolved)

## Open Questions

- How does the agent balance exploration (trying new approaches) vs exploitation (refining what works)?
- What's the right budget granularity — per-investigation? per-branch? per-session?
- When should the agent create parallel branches vs iterate sequentially?
- How does the qino-label write-back surface integrate — annotations on eval nodes? structured judgments in data/?
- What does convergence look like? When does the agent recommend "promote this to production"?