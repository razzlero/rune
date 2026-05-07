---
name: ask-pi
description: Delegate a question or task to pi coding agent in headless mode and return the result
---

# ask-pi

Run pi non-interactively as a subprocess and return its answer. Useful for fresh-context second opinions, parallel fan-out (e.g. lens-based reviewers), or tasks where a clean session beats the current one.

## Invocation

```bash
pi -p "<prompt>"
```

Default text mode prints only the final assistant message — no tool calls, no thinking, no transcript — so the parent agent gets just the answer.

For longer prompts, pipe via stdin:

```bash
echo "<prompt>" | pi -p
```

## Useful flags

- `--model <pattern>` — pick a specific model (e.g. `sonnet`, `opus`, `gpt-5.4`)
- `--no-session` — don't persist a session file (ephemeral one-shot)
- `--no-tools` / `-nt` — disable all tools when you only want a model answer
- `--tools <names>` / `-t` — comma-separated allowlist (e.g. `-t Read,WebSearch`)
- `--mode json` — emit the full event stream as JSONL on stdout (parse for the final assistant message; only use when text mode isn't enough)

## Notes

- Parallelism: launch N independent calls in one parent turn (parallel Bash invocations) for fan-out — each is a fresh process with no shared state.
