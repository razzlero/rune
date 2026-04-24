# rune — pi package

Rune workflows for [pi](https://pi.dev/).

## Setup

### Prerequisite: a subagent extension

Several rune skills are instructed to run subagents, so make sure to install a subagent extension. For example:

```bash
pi install npm:@tintinweb/pi-subagents
```

See [`@tintinweb/pi-subagents`](https://github.com/tintinweb/pi-subagents). Skill prose is tool-name-agnostic, so other subagent extensions work too — swap later without touching skill prose.

### Install the package

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

