# Codex provider

## Invocation

`codex exec` writes the final assistant message to stdout. Session/progress text goes to stderr, so discard or redirect stderr when you want final-output-only behavior:

```bash
codex exec "<prompt>" 2>/dev/null
```

For longer prompts, pipe via stdin:

```bash
echo "<prompt>" | codex exec - 2>/dev/null
```

## Useful flags

- `-m, --model <name>` — pick a specific model
- `--json` — emit events as JSONL on stdout instead of plain final-message output
- `-o, --output-last-message <file>` — also write the final message to a file
- `-s, --sandbox read-only` — restrict to read-only for pure read/search tasks
- `--skip-git-repo-check` — run outside a git repo
- `--ephemeral` — don't persist a session file

## Notes

- Plain `codex exec` already puts the final message on stdout; the extra session/status output is on stderr.
- Redirect stderr when you need final-output-only behavior. Use your shell's stderr redirection syntax on non-POSIX shells.
- Default sandbox is `workspace-write`; pass `-s read-only` for pure read/search tasks.
