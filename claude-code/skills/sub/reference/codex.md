# Codex provider

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
- `--json` — emit events as JSONL on stdout
- `-o, --output-last-message <file>` — write only the final message to a file
- `-s, --sandbox read-only` — restrict to read-only for pure read/search tasks
- `--skip-git-repo-check` — run outside a git repo
- `--ephemeral` — don't persist a session file

## Notes

- Default sandbox is `workspace-write`; pass `-s read-only` for pure read/search tasks.
