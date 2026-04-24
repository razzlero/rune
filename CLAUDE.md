# Working in this repo

This repo contains skills for AI-assisted development workflows.

## Keep skill descriptions short

Skill `description` frontmatter shows up as autocomplete hits — keep it to a single short line (roughly under ~120 chars). State what it does; leave details, flags, and side notes for the SKILL.md body.

## Record external influences

When you encounter an external project, tool, or writeup (blog post, repo, paper) that lives in rune's problem space, add an entry to [`REFERENCES.md`](REFERENCES.md). It doesn't need to have changed a skill yet — adjacent prior art is worth keeping. One-line bullet per source: link + 1-2 sentences on what it offers. Place under the category that fits — **AI skills, plugins & tools**, **Writeups & posts**, or **Research & guides**. Don't record "what it influenced" — those bindings decay as skills change.

## Per-repo state

Plugin state lives under `<repo>/rune/plans/`, `rune/retros/`, `rune/musings/` — plugin-owned output dirs, committed, created lazily on first use by the relevant skill.

Skills hard-default to `rune/<subdir>/` — no config file. If a consuming repo needs a different directory (e.g. a pre-existing `docs/plans/`), the repo's own CLAUDE.md instructs the skill in-context; the plugin itself doesn't know about overrides.

When adding a new path-using feature, default to `rune/<subdir>/` and state the default in the skill's prose.
