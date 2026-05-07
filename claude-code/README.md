# rune — Claude Code plugin

[Claude Code skills](https://docs.anthropic.com/en/docs/claude-code/skills) for the full development lifecycle — plan, execute, review, reflect. Part of the [rune](../README.md) repo.

If you're building your own skills, the [`skills/`](skills/) directory is probably the most useful part to read — each `SKILL.md` is a self-contained example of one shape of agent-assisted workflow.

## Installation

Install is local-only. Clone the rune repo somewhere, then point Claude Code at this folder:

```
/plugin marketplace add <path-to-rune>/claude-code
/plugin install rune@rune
```

Then `/reload-plugins` (or restart Claude Code) if the commands don't show up immediately.

## Skills

Skill names are verbs — actions you tell the AI to do, like "craft a plan" or "temper this diff". The command reads like an instruction, and the skill body is the "how". Keeping each step behind its own command also means you decide when the AI moves on, instead of it jumping ahead too early.

### Development lifecycle

| Skill | Description |
|-------|-------------|
| [muse](skills/muse/SKILL.md) | Scratchpad + thinking partner — explore a ticket/problem, rubber-duck ideas, capture raw thoughts into a durable musing doc. Optional pre-planning step upstream of /craft. |
| [craft](skills/craft/SKILL.md) | Plan a change — durable planning doc covering one unit of work |
| [refine](skills/refine/SKILL.md) | Refine a plan by stress-testing it from the no-context implementer's perspective — single-pass comprehension audit with diverse reviewer lenses |
| [forge](skills/forge/SKILL.md) | Execute a plan with TDD discipline — red, green, refactor |
| [temper](skills/temper/SKILL.md) | Iterative fix-and-review loop on code changes until all models agree it's clean |
| [reflect](skills/reflect/SKILL.md) | Capture non-obvious project knowledge into `rune/lore/` — architecture rationale, conventions, "where does X live" maps. Other skills consult lore when their work touches a relevant topic. Opportunistic, not required. |

Typical flow: `muse` → `craft` → `refine` → `forge` → `temper` → `reflect`. Each step is a separate slash command so the user picks what the work needs — skip, repeat, or reorder as appropriate (e.g. straight to `/forge` for trivial work, or several `/temper` rounds on a gnarly diff).

### Utilities

| Skill | Description |
|-------|-------------|
| [panel](skills/panel/SKILL.md) | Review code changes with parallel model reviewers and adversarial validation. Best for reviewing others' code. *(Needs improvement — I'll keep iterating on it.)* |
| [distill](skills/distill/SKILL.md) | Compact a doc to preserve meaning while cutting tokens and redundant instructions |
| [sub](skills/sub/SKILL.md) | Delegate a question or task to a fresh pi or Codex subagent — second opinion from a different model/toolset |

Plugin-namespaced invocation (`rune:reflect`, `rune:craft`, etc.) is available when a bare name collides with something else.

## Output directories

Skills write output under `rune/<subdir>/` in the consuming repo — this is a cross-tool convention, documented in the [root README](../README.md#output-directories).
