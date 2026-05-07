---
name: ask-claude
description: Delegate a question or task to Claude Code in headless mode and return the result
---

# ask-claude

Run Claude Code non-interactively as a subprocess and return its answer. Useful for web search, second opinions, or tasks where Claude's tools or strengths fit better than the current session.

## Invocation

```bash
claude -p "<prompt>" --output-format json
```

Read the prompt from the user, run the command, parse the `result` field from the JSON response, and return it.

For longer prompts, pipe via stdin:

```bash
echo "<prompt>" | claude -p --output-format json
```

## Useful flags

- `--model <name>` — pick a specific model (e.g. `sonnet`, `opus`)
- `--allowedTools "WebSearch WebFetch"` — restrict to specific tools when you only want search/fetch behavior

## Notes

- Output: when `--output-format json`, parse `result` for the final text.
