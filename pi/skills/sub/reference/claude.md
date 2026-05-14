# Claude provider

## Invocation

```bash
claude -p "<prompt>" --output-format json
```

Parse the `result` field from the JSON response and return it.

For longer prompts, pipe via stdin:

```bash
echo "<prompt>" | claude -p --output-format json
```

## Useful flags

- `--model <name>` — pick a specific model
- `--allowedTools "WebSearch WebFetch"` — restrict tool access when needed
