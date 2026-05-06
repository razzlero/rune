---
name: reflect
description: Capture non-obvious project knowledge into rune/lore/ for future sessions to consult
---

# reflect

Maintain a small set of AI-readable docs about the project under `rune/lore/`. Lore is a topical reference future sessions can consult when working in a relevant area — architecture, cross-cutting conventions, "where does X live" maps, domain glossary, "watch out for" gotchas. Recording the intended architecture is the highest-value job in inconsistent codebases — without it, fresh AI sessions pattern-match on whatever code they read first and propagate the inconsistency.

Lore reflects the project's **current state**. The job is curation, not transcription — tighten, replace, or delete entries that no longer match reality. Most invocations should produce small targeted edits or no edits at all.

## Lore directory & first-run bootstrap

Lore lives at `rune/lore/`. On invocation, create the folder and starter files if they don't exist.

Create `rune/lore/INDEX.md`:

```markdown
# Lore index

AI-maintained context for this project. Each entry below is a topic doc covering non-obvious knowledge that future sessions need but can't easily re-derive from a code skim. Maintained by `/reflect`.

## Topics

<!-- one line per topic doc:
- [<topic>](<filename>.md) — <one-line description>
-->
```

Create `rune/CLAUDE.md` if missing:

```markdown
# rune-managed context

This directory contains AI-maintained project context under `lore/`. When working on a topic that may have non-obvious context (architecture, conventions, gotchas), check [`lore/INDEX.md`](lore/INDEX.md) for a relevant doc.
```

If `rune/CLAUDE.md` already exists, **append** the lore pointer rather than overwriting — the file may carry other rune-related instructions.

## When to invoke

Opportunistic. Invoke after meaningful work that surfaced non-obvious knowledge, after learning something about the codebase that future sessions would benefit from, or as periodic maintenance. Not a required step in any workflow — skip when the work didn't produce anything lore-worthy.

**Strong candidates**: places where the user corrected the agent's direction — initial plan proposals that needed redirection, forged changes that took the wrong approach, recurring nudges away from the same mistake. Corrections signal context the agent was missing, which is what lore would have provided. Not every correction generalizes — capture the ones that would help any future session, not one-off mistakes.

## Drafting loop

1. **Read existing lore.** Open `rune/lore/INDEX.md` and any topic docs that look relevant. Treat them as the prior state to extend or correct.
2. **Identify candidates** from recent session context and any args. Apply the scope rules below — the candidate list is usually longer than the actual update.
3. **Decide for each**: update existing doc, create new, or skip. *Skip is the most common answer.*
4. **When updating**: edit in place. Tighten, replace, delete outdated content. Don't append changelogs or dated sections.
5. **When creating**: kebab-case filename describing the topic (`auth-flow.md`, `error-handling.md`). Add a one-line entry to INDEX.md.
6. **Tidy INDEX.md** if entries have grown stale, unclear, or out of order.
7. **Stop when there's nothing left to capture or tidy.** A no-change run is a valid outcome.

## What to capture

- **Architecture** — the system's structural shape (layers, services, key components, subsystem boundaries, how data flows, the canonical pattern for common changes) and the rationale behind it. The single most valuable thing lore records, especially when the codebase is inconsistent and the architecture isn't obvious from any one example.
- Cross-cutting conventions that aren't visible in any one file.
- "Where does X live" maps for non-obvious organization.
- Domain glossary — terms that mean something specific in this project.
- External references — links to design docs, RFCs, tickets, blog posts, or specs that informed the project. Hard to rediscover later.
- "Watch out for" gotchas, footguns, deceptive simplifications.

## What NOT to capture

- File-level summaries, function lists, change logs — the file, `grep`, and `git log` are authoritative.
- Anything obvious from a 10-minute repo skim.
- Conversation transcripts or session play-by-play — lore is a knowledge base, not a journal.
- Speculative or "might be useful" entries — capture validated knowledge, not hypothetical context.

## Anti-patterns

- **Output every invocation.** If most reflect runs add or change something, the discipline is drifting — apply the scope filter, don't find something to say.
- **Docs that grow past a couple of pages.** Long docs bury the signal and accumulate stale claims. Split or trim.
- **Treating lore as a log.** Rewrite in place; lore is the current view, not annotated history.
