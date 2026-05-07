# pi provider

## Invocation

```bash
pi -p "<prompt>"
```

Default text mode prints only the final assistant message.

For longer prompts, pipe via stdin:

```bash
echo "<prompt>" | pi -p
```

## Useful flags

- `--model <pattern>` — pick a specific model
- `--no-session` — use an ephemeral one-shot session
- `--no-tools` / `-nt` — disable all tools when you only want a model answer
- `--tools <names>` / `-t` — comma-separated allowlist
- `--mode json` — emit JSONL events when text mode is not enough
