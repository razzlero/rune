## Rune-managed context

If `rune/CLAUDE.md` exists it contains context relevant to the `rune` plugin skills. You should read it when planning or musing.

## Subagent delegation

Use the `sub` skill to launch subagents. Default to `pi` unless asked for `claude` or `codex`.

Use subagents when they have capabilities this agent lacks or when explicitly asked for them.

- Web search: delegate to `claude` or `codex`; use `codex` by default.
- Web fetch: for webpage fetching or extraction beyond `curl`, delegate to `claude` or `codex`; use `codex` by default.
- PDF reading: `read` can't open PDFs; delegate to `claude` or `codex`; use `codex` by default.
