---
name: sub
description: Delegate a question or task to a fresh pi or Codex subagent
---

# sub

Launch a fresh headless subagent and return its final answer.

Use `pi` by default. Switch providers when the user asks for one explicitly or the task benefits from a different tool/model family.

Claude Code already has built-in subagents, so this skill is for external providers only.

## Provider choice

- `pi` — default; best general-purpose external subagent in this package
- `codex` — use when the user specifically wants Codex CLI

## Process

1. Identify the provider (`pi` if unspecified).
2. Read the matching reference file:
   - [reference/pi.md](reference/pi.md)
   - [reference/codex.md](reference/codex.md)
3. Run the provider in headless mode.
4. Return only the provider's final answer unless the user asked for orchestration details.

## Notes

- Each call is a fresh process with no shared state.
- For parallel fan-out, launch multiple independent subprocesses in one parent turn.
- Prefer Claude Code's built-in subagents when an in-process subagent is sufficient.
