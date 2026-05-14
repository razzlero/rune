---
name: sub
description: Delegate a question or task to a fresh pi or Codex subagent
---

# sub

Launch a fresh headless subagent and return its final answer.

Use `pi` by default. Switch to `codex` when the user asks for it explicitly or the task benefits from its tool/model family.

## Provider choice

- `pi` — default; best general-purpose fresh-context subagent in this package
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
- Prefer provider-native text output when it already returns just the final answer; otherwise parse the final message as described in the reference file.
