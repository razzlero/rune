# References

A curated library of skill collections, tools, and writeups worth learning from — adjacent prior art in rune's problem space. Entries describe what the source offers; how rune uses it shifts over time, so those bindings aren't recorded here.

## AI skills, plugins & tools

- [superpowers](https://github.com/obra/superpowers) — Claude Code skill collection with systematic debugging, verification-before-completion, severity-gated reviews, and a `writing-plans` skill built around file structure + TDD tasks.
- [GSD (get-shit-done)](https://github.com/gsd-build/gsd-2) — Spec-driven, meta-prompting workflow for Claude Code: 29 slash-command skills plus specialized research/planning/execution/verification subagents. Pitch is context-rot prevention via subagent isolation — each plan runs in a fresh 200k-token context. Frequently paired with superpowers ("GSD stabilizes, superpowers executes").
- [planning-with-files](https://github.com/OthmanAdi/planning-with-files) — Persistent markdown-based planning using the filesystem as working memory; splits `task_plan.md` / `findings.md` / `progress.md`.
- [napkin](https://github.com/blader/napkin) — Scratchpad skill built around curated, prioritized findings rather than chronological logs.
- [caveman](https://github.com/juliusbrussee/caveman) — Token compression via drop-hedging, drop-filler, drop-restatement rules.
- [agent-council](https://github.com/yogirk/agent-council) — Convenes a panel of CLI-based AI agents (Claude Code, Codex, Gemini CLI) for structured multi-stage deliberation.
- [llm_council_skill](https://github.com/shuntacurosu/llm_council_skill) — Multi-LLM council with peer review and chairman synthesis.
- [cc-skills](https://github.com/rube-de/cc-skills) — Curated skill collection including an AI council code-review plugin and project-management tooling.
- [ceo-review-skill](https://github.com/yahav123147/ceo-review-skill) — "Temporal interrogation" pattern (hour-1 / hours 2-3 / hours 4-5 predictions) and adversarial inversion framing.
- [wrsmith108/plan-review-skill](https://github.com/wrsmith108/plan-review-skill) — Multi-perspective parallel reviewers using different role lenses, each blind to the others, with deduplicated output.
- [fdev-llc/plan-review-skill](https://github.com/fdev-llc/plan-review-skill) — Bounded checklist + verdict line + severity tags + explicit "diminishing returns, approve when solid enough" stop language.
- [github/spec-kit](https://github.com/github/spec-kit) — Spec-Driven Development toolkit: `spec.md` / `plan.md` / `tasks.md` artifact split, with constitution-check gating and parallel-task markers.
- [AWS Kiro](https://kiro.dev/docs/specs/) — Spec-driven IDE: `requirements.md` (EARS notation) / `design.md` / `tasks.md`, with a bugfix variant.
- [claude-task-master](https://github.com/eyaltoledano/claude-task-master) — PRD-to-task-graph workflow with explicit dependencies and parallelism.
- [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) — Multi-artifact agile-agent workflow with role-separated PRD / architecture / stories authored by different agents.
- [NotMyself plan optimizer](https://gist.github.com/NotMyself/09cc37ae457be1009aba4b9ae23249eb) — Three-level progressive-disclosure plan structure (Overview → Phase summaries → detailed steps).

## Writeups & posts

- [the grug brained developer](https://grugbrain.dev/) — "many word bad, few word good." Original namesake of `distill` (previously `grugify`); the spirit outlasted the name.
- [Harper Reed — my LLM codegen workflow](https://harper.blog/2025/02/16/my-llm-codegen-workflow-atm/) — `spec.md` / `prompt_plan.md` / `todo.md` separation; plan is a sequence of TDD prompts.
- [Simon Willison — my LLM codegen workflow](https://simonwillison.net/2025/Feb/21/my-llm-codegen-workflow-atm/) — Companion post, same spec → plan → todo shape.
- [ghuntley — Ralph](https://ghuntley.com/ralph/) — Specs are durable; `IMPLEMENTATION_PLAN.md` is disposable and regenerated each loop. Anthropic's `ralph-wiggum` plugin is a minimal companion.
- [Addy Osmani — good-spec](https://addyosmani.com/blog/good-spec/) — Non-Goals as first-class for LLMs; "AI cannot infer from omission."
- [Cursor — my plan template](https://forum.cursor.com/t/my-plan-template/43568) — Plan template with explicit Assumptions, Confidence Level, and Questions sections.

## Research & guides

- [Anthropic — skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — Consistent terminology, challenge every explanatory clause, avoid option-lists without a default, third-person triggerable descriptions, redundancy weakens rather than reinforces.
- [Selective Context](https://github.com/liyucheng09/Selective_Context) / [LLMLingua](https://github.com/microsoft/LLMLingua) — Research on prompt compression via self-information pruning: tokens predictable from context carry no new information.
