# qino Chronicles — Public App Idea

**Captured:** 2025-12-08

## The Impulse

Two chronicles exist:
- **qino-claude/chronicle/** — the tool development journey (4 chapters). Characters: the wanderer, the assistant, the six voices, the listener, the world. About tools learning to help humans think.
- **concepts-repo/chronicle/** — the concept development journey (2 chapters). Locations: the gathering-hall, the quiet room, the keeper's table, discovery-grid. About ideas taking shape.

The chronicles live in separate repositories but share a world. They could be read together — two perspectives on the same evolving ecosystem.

## What I'm Imagining

A simple public app that renders the chronicles side by side (or navigable):

- **Two nav items** in the main header — one for each chronicle
- **No login** — these are public artifacts
- **Distinctive background colors** for each chronicle — so you know which world you're in
- **Renders markdown** — the chapter files, as they are
- **Low complexity** — just a reader, nothing more

## Open Question: Publishing

How would chapters get from the repositories to the app?

Possibilities:
- Manual copy
- A `/scribe:publish` command that automates the flow
- GitHub actions that sync on push
- Reading directly from GitHub raw files

The workflow should stay low complexity. The chronicles are written infrequently (weekly at most). Manual or semi-manual is fine.

## What This Connects To

- **qino-scribe** — the tool that writes the chronicles
- **The wanderer's journey** — this would make the journey publicly accessible
- **Future collaborators** — a way to share the ecosystem's story without explaining everything
