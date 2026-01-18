# Qinolabs Workspace

Multi-repo workspace for the qino ecosystem.

## What This Is

This **workspace repository** versions **coordination between child repos**, not their content.

Each child repository (`qinolabs-repo/`, `concepts-repo/`, etc.) has its own `.git/` and is independently versioned. This workspace repo tracks:

- **Which repos exist** (`workspace-manifest.json`)
- **How repos relate** (`workspace-config.json`)
- **Cross-repo navigation** (`CLAUDE.md`)

## Repositories

| Repo | Type | Purpose |
|------|------|---------|
| `qinolabs-repo/` | implementation | Main monorepo with all apps |
| `qino-claude/` | tooling | Claude Code plugins and agents |
| `concepts-repo/` | concepts | Concept exploration and vision |
| `qino-research/` | research | Research and investigations |
| `qino-lingo/` | archives | Conversation archives |
| `external-apps/` | implementation | Client project apps |
| `external-concepts/` | concepts | Client project concepts |

See `workspace-manifest.json` for complete registry.

## Structure

```
qinolabs/                          # Workspace root (THIS repo)
├── .git/                          # Workspace version control
├── .gitignore                     # CRITICAL: ignores all child repos
├── README.md                      # This file
├── CLAUDE.md                      # Navigation across repos
├── workspace-manifest.json        # Registry of all repos
├── workspace-config.json          # Cross-repo links
├── .claude/
│   └── qino-config.json           # Workspace-level qino config
│
├── qinolabs-repo/                 # Child repo (independently versioned)
├── qino-claude/                   # Child repo (independently versioned)
├── concepts-repo/                 # Child repo (independently versioned)
└── ...                            # Other child repos
```

## Key Files

### `workspace-manifest.json`
Registry of all repos - what exists, their types, purposes.

Query this to discover available repos without hardcoding.

### `workspace-config.json`
Cross-repo coordination:
- Repo paths (relative only)
- Concept ↔ implementation links
- MCP server configs

### `CLAUDE.md`
Navigation guide for Claude - which repo to use for which task.

### `.claude/qino-config.json`
Workspace-level qino configuration. Tells qino skill this is a multi-repo workspace.

## Critical Principle

**This repo versions RELATIONSHIPS, not CONTENT.**

✅ Version:
- workspace-manifest.json (repo registry)
- workspace-config.json (cross-repo links)
- CLAUDE.md (navigation)
- .gitignore (critical!)
- Documentation about workspace structure

❌ Never version:
- Child repos (they're in .gitignore)
- Machine-specific configs (.mcp.json with absolute paths)
- Generated files (AGENTS.md)
- Conversation archives

## Analogy

Like `pnpm-workspace.yaml` knows which packages exist but doesn't contain the packages themselves, this workspace repo knows which repos exist but doesn't contain their code.

## Getting Started

**For development:**
```bash
cd qinolabs-repo && cat CLAUDE.md
```

**For concept work:**
```bash
cd concepts-repo && cat CLAUDE.md
```

**For research:**
```bash
cd qino-research && cat CLAUDE.md
```

See `CLAUDE.md` for complete navigation guide.

## Maintenance

**Adding a new repo:**
1. Create the repo in its own directory
2. Add entry to `workspace-manifest.json`
3. If it links to concepts, add to `workspace-config.json` conceptLinks
4. Update `.gitignore` to ignore the new directory
5. Commit workspace changes

**Removing a repo:**
1. Remove from `workspace-manifest.json`
2. Remove from `workspace-config.json` if present
3. Remove from `.gitignore`
4. Commit workspace changes
5. (Optionally archive the actual repo directory elsewhere)

---

**Last Updated:** 2025-01-18
