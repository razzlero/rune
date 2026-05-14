# rune — pi package

Rune workflows for [pi](https://pi.dev/).

## Setup

Install the package into pi:

```bash
pi install ./pi
```

Then restart pi or run:

```text
/reload
```

Invoke the current skills as native pi skill commands:

```text
/skill:muse
/skill:craft
/skill:refine
/skill:forge
/skill:temper
/skill:reflect
/skill:panel
/skill:distill
/skill:sub
```

Skills that fan out to "fresh subagents" (`panel`, `temper`, `refine`) can use `/skill:sub` for headless delegation — no extension needed. Skill prose is tool-name-agnostic, so the same skills work in the [claude-code](../claude-code) package via `Task` and in any future Codex package via `codex exec`.

## Development

This package keeps its own dev tooling locally:

```bash
cd pi
npm install
npm run check
```

Or from the repo root:

```bash
npm --prefix pi install
npm --prefix pi run check
```

Current setup:
- Biome for formatting/linting
- TypeScript for typechecking `**/*.ts`
- no build step; pi loads extensions directly

## Contents

| Path | Purpose |
|------|---------|
| [`package.json`](package.json) | pi package manifest |
| [`skills/`](skills/) | Baseline copies of the current rune skills |
| [`extensions/`](extensions/) | Pi extensions, one subfolder per extension. [`guidance`](extensions/guidance/) appends [`guidance.md`](extensions/guidance/guidance.md) to the system prompt on every turn. |

## Current skill set

| Skill | Description |
|-------|-------------|
| [muse](skills/muse/SKILL.md) | Capture raw thoughts into an organized musing doc — pre-planning scratchpad and thinking partner |
| [craft](skills/craft/SKILL.md) | Craft a plan that captures everything an implementer needs to hit the ground running |
| [refine](skills/refine/SKILL.md) | Refine a plan by stress-testing it from the no-context implementer's perspective |
| [forge](skills/forge/SKILL.md) | Execute a plan with TDD discipline — red, green, refactor |
| [temper](skills/temper/SKILL.md) | Iterative fix-and-review loop on code changes until all models agree it's clean |
| [reflect](skills/reflect/SKILL.md) | Capture non-obvious project knowledge into `rune/lore/` for other skills to consult on future work |
| [panel](skills/panel/SKILL.md) | Review code changes with parallel model reviewers and adversarial validation |
| [distill](skills/distill/SKILL.md) | Compact a doc to preserve meaning while cutting tokens and redundant instructions |
| [sub](skills/sub/SKILL.md) | Delegate a question or task to a fresh pi, Claude, or Codex subagent |

