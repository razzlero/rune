# rune — pi package

Rune workflows adapted for pi.

## Status

This directory is now a local pi package. Today it ships the baseline rune skill set under [`skills/`](skills/); later it can also grow pi-native extensions, prompts, and themes without changing the install shape.

Shared conventions like writing output under `rune/<subdir>/` still apply unless a consuming repo overrides them in its own agent instructions.

That means:

- `pi/` is maintained as its own pi package
- skills here can evolve toward pi-specific prompts, extensions, and packaging
- changes should be made intentionally in this folder rather than assuming any automatic sync from elsewhere

## Setup

### Prerequisite: tmux

For now, assume tmux is available. Some rune skills for pi — especially `refine`, `panel`, and `temper` — use separate fresh pi sessions in tmux panes/windows as the stand-in for subagents.

Pi's tmux notes recommend adding this to `~/.tmux.conf`:

```tmux
set -g extended-keys on
set -g extended-keys-format csi-u
```

Then restart tmux fully:

```bash
tmux kill-server
tmux
```

### Install the package

Install the package directly:

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
/skill:retro
/skill:panel
/skill:distill
```

## Contents

| Path | Purpose |
|------|---------|
| [`package.json`](package.json) | pi package manifest |
| [`skills/`](skills/) | Baseline copies of the current rune skills |

## Current skill set

| Skill | Description |
|-------|-------------|
| [muse](skills/muse/SKILL.md) | Capture raw thoughts into an organized musing doc — pre-planning scratchpad and thinking partner |
| [craft](skills/craft/SKILL.md) | Craft a plan that captures everything an implementer needs to hit the ground running |
| [refine](skills/refine/SKILL.md) | Refine a plan by stress-testing it from the no-context implementer's perspective |
| [forge](skills/forge/SKILL.md) | Execute a plan with TDD discipline — red, green, refactor |
| [temper](skills/temper/SKILL.md) | Iterative fix-and-review loop on code changes until all models agree it's clean |
| [retro](skills/retro/SKILL.md) | Reflect on completed work and maintain durable lessons |
| [panel](skills/panel/SKILL.md) | Review code changes with parallel model reviewers and adversarial validation |
| [distill](skills/distill/SKILL.md) | Compact a doc to preserve meaning while cutting tokens and redundant instructions |

## Notes

The package currently exports the skill set via the `pi` manifest in [`package.json`](package.json). Extensions, prompts, and themes can be added later under this same package.
