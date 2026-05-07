## Subagent delegation

The skills `ask-pi`, `ask-claude`, and `ask-codex` each spawn a fresh agent in headless mode and return its final answer. Treat them as your subagent mechanism — when instructed you to launch subagents use these skills. Default to `ask-pi` unless specifically asked to use a Claude or Codex subagent.
