## Subagent delegation

The skill `sub` spawns a fresh agent in headless mode and returns its final answer. Treat it as your subagent mechanism — when instructed to launch subagents use this skill. Default to the `pi` provider unless specifically asked to use `claude` or `codex`.

### Web search

The claude and codex subagents have the ability to perform web searches. When asked to perform a web search delegate the task to either claude or codex. Use codex for web search by default.

### Web fetch

Claude and codex subagents can fetch a specific URL and intelligently extract its content (including JS-rendered pages). When asked to read a webpage or pull content from a URL beyond what `curl` can give you, delegate to claude or codex. Use codex by default.

### PDF reading

Your `read` tool can't open PDFs. When asked to read a PDF, delegate to a claude subagent — its read tool handles PDFs natively.
