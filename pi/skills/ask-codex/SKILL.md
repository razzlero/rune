---
name: ask-codex
description: Delegate a question or task to Codex CLI in headless mode and return the result
---

# ask-codex

Run Codex CLI non-interactively as a subprocess and return its answer. Useful for web search, second opinions, or tasks where Codex's tools or strengths fit better than the current session.

## Invocation

```bash
codex exec "<prompt>"
```

For structured output, write the final message to a file:

```bash
codex exec -o /tmp/codex-out.txt "<prompt>"
```

For longer prompts, pipe via stdin:

```bash
echo "<prompt>" | codex exec -
```

## Useful flags

- `-m, --model <name>` — pick a specific model
- `--json` — emit events as JSONL on stdout (for parsing)
- `-o, --output-last-message <file>` — write only the final message to a file
- `-s, --sandbox read-only` — restrict to read-only when the task shouldn't touch the filesystem
- `--skip-git-repo-check` — run outside a git repo
- `--ephemeral` — don't persist a session file

## Notes

- Latency: seconds to tens of seconds. Not for tight loops.
- Default sandbox is `workspace-write` — pass `-s read-only` for pure read/search tasks.
