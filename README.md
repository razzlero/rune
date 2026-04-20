# rune

A personal collection of AI-assisted development workflows, packaged per-tool.

## About this repo

Rune is a personal project, shared openly for **inspiration and learning**. It reflects my ongoing experimentation and learning, alongside studying other people's plugins, skill collections, and writeups (see [`REFERENCES.md`](REFERENCES.md)) — distilling what works for my workflow into something I actually use every day.

Some things to know before using it:

- **Personal project.** This repo is shaped around my own taste and workflow. Forks and feedback are welcome. But PRs are unlikely to be merged unless they happen to align with my own goals.
- **Frequent breaking changes.** Skills get renamed, merged, split, or deleted whenever I find a better shape. There's no deprecation policy and no guarantee today's commands exist tomorrow.
- **Best used as a base, not a dependency.** If any of this is useful to you, the recommended path is to **fork the repo** (or copy individual skills into your own setup) and bend it to your own workflow. Treat rune as a reference implementation, not a product.
- **Install is local-only.** Each tool folder is meant to be installed from a local clone of this repo, not via a remote git URL. This keeps each tool's layout idiomatic without fighting any particular tool's packaging conventions. See each tool folder's README for the exact install step.

## Tools

Each tool lives in its own top-level folder, self-contained with its own manifest and README.

| Tool | Location |
|------|----------|
| Claude Code plugin | [`claude-code/`](claude-code/README.md) |

More tool folders will appear here as I port the workflows to other AI coding surfaces (pi, Codex, etc.).

## Shared conventions

### Output directories

Skills across all tools write their output under `rune/<subdir>/` in the *consuming* repo (i.e. wherever you're running the agent, not this repo):

| Dir | Skill | Contents |
|-----|-------|----------|
| `rune/plans/` | `/craft` | One plan per change |
| `rune/musings/` | `/muse` | Pre-planning scratchpad, rubber-duck, thought capture |
| `rune/retros/` | `/retro` | Reflections on completed work (Keep/Problem/Try) + `LESSONS.md` index |

Each dir is created on demand by its owning skill. A repo that already has a different layout (e.g. a pre-existing `docs/plans/`) can instruct the relevant skill in its own `CLAUDE.md` — "when `/craft` runs here, write plans to `docs/plans/` instead." Tools don't read a config file for this; the CLAUDE.md equivalent is already in context, so the LLM honors the override from there.

The `rune/` directory at the root of *this* repo is just the same convention applied to the plugin's own work — retros, lessons, and plans about rune itself.

## Structure

```
rune/                            — multi-tool monorepo
├── REFERENCES.md                — related skill collections and writeups worth learning from
├── rune/                        — this repo's own retros / plans / lessons (same convention skills use in consuming repos)
│   ├── musings/
│   ├── plans/
│   └── retros/
└── claude-code/                 — Claude Code plugin (see its README)
```

## References

A curated library of related skill collections and writeups worth learning from — some directly informed rune, others are kept around as future inspiration. See [`REFERENCES.md`](REFERENCES.md).

## License

MIT — see [`LICENSE`](LICENSE).
