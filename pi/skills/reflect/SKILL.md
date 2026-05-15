---
name: reflect
description: Capture non-obvious project knowledge into rune/lore/ for future sessions to consult
---

# reflect

Maintain a small set of AI-readable docs about the project under `rune/lore/`. Lore is a topical reference future sessions can consult when working in a relevant area — architecture, cross-cutting conventions, "where does X live" maps, domain glossary, "watch out for" gotchas. Recording the intended architecture is the highest-value job in inconsistent codebases — without it, fresh AI sessions pattern-match on whatever code they read first and propagate the inconsistency.

Lore reflects the project's **current state**. The job is curation, not transcription — tighten, replace, or delete entries that no longer match reality. Most invocations should produce small targeted edits or no edits at all.

A reflect run also checks for **retrieval failures**. If the session missed, re-derived, or got corrected on something that already lives in lore, improve discoverability instead of duplicating content. Tighten filenames, headings, cross-links, and the `rune/CLAUDE.md` index so future sessions are more likely to find the right note in time.

## Lore directory & first-run bootstrap

Lore lives at `rune/lore/`. On invocation, create the folder and starter files if they don't exist.

Update `rune/CLAUDE.md` in place so it remains a concise guide to `rune/` with a compact lore topic index and, when justified, a small amount of must-know context.

If `rune/CLAUDE.md` does not exist yet, create it using [reference/rune-claude-template.md](reference/rune-claude-template.md) as the starter shape.

Adapt the result to the repo's actual conventions before writing. If the repo keeps plans outside `rune/`, point to the real plan location.

## When to invoke

Opportunistic. Invoke after meaningful work that surfaced non-obvious knowledge, after learning something about the codebase that future sessions would benefit from, or as periodic maintenance. Not a required step in any workflow — skip when the work didn't produce anything lore-worthy.

**Strong candidates**: places where the user corrected the agent's direction — initial plan proposals that needed redirection, forged changes that took the wrong approach, recurring nudges away from the same mistake. Corrections signal context the agent was missing, which is what lore would have provided. Not every correction generalizes — capture the ones that would help any future session, not one-off mistakes.

## Drafting loop

1. **Read existing rune guidance and lore.** Open `rune/CLAUDE.md` and any `rune/lore/` topic docs that look relevant. Treat them as the prior state to extend or correct.
2. **Identify candidates** from recent session context, any args, and the existing lore you just read. Look for both new knowledge and retrieval failures: relevant lore that existed already but was not surfaced when it would have helped.
3. **Decide for each**: update existing doc, improve discoverability, create new, or skip. *Skip is the most common answer.*
4. **When updating content**: edit in place. Tighten, replace, delete outdated content. Don't append changelogs or dated sections.
5. **When discoverability is the problem**: prefer retrieval fixes over duplicate prose — rename vague files, sharpen titles/opening lines, add or improve cross-links, and rewrite the `rune/CLAUDE.md` lore index entry so it advertises the terms and situations a future agent will actually look for.
6. **When creating**: use a kebab-case filename describing the topic (`auth-flow.md`, `error-handling.md`). Update `rune/CLAUDE.md` so its lore section lists the topic doc with a one-line description that says what it covers and when to read it, giving future agents a no-search map to relevant context.
7. **Keep must-know context exceptional.** `rune/CLAUDE.md` may include a few broadly relevant points agents should almost always know up front, but only when burying them in lore would make future sessions miss them. Prefer links to lore for everything else.
8. **Tidy `rune/CLAUDE.md`** if its directory guide, must-know context, or lore topic index has grown stale, unclear, too detailed, or inconsistent with repo-specific paths.
9. **Stop when there's nothing left to capture or tidy.** A no-change run is a valid outcome.

## What to capture

- **Architecture** — the system's structural shape (layers, services, key components, subsystem boundaries, how data flows, the canonical pattern for common changes) and the rationale behind it. The single most valuable thing lore records, especially when the codebase is inconsistent and the architecture isn't obvious from any one example.
- Cross-cutting conventions that aren't visible in any one file.
- "Where does X live" maps for non-obvious organization.
- Domain glossary — terms that mean something specific in this project.
- External references — links to design docs, RFCs, tickets, blog posts, or specs that informed the project. Hard to rediscover later.
- "Watch out for" gotchas, footguns, deceptive simplifications.
- Retrieval hints when needed — the terms, aliases, adjacent concepts, or "read this when..." cues that make the right lore doc easier to find.

## What NOT to capture

- File-level summaries, function lists, change logs — the file, `grep`, and `git log` are authoritative.
- Anything obvious from a 10-minute repo skim.
- Conversation transcripts or session play-by-play — lore is a knowledge base, not a journal.
- Speculative or "might be useful" entries — capture validated knowledge, not hypothetical context.

## Indexing for retrieval

- Treat the `rune/CLAUDE.md` lore section as a **retrieval index**, not just an inventory. Each entry should help a future agent recognize both the topic and the moment to open it.
- Prefer index lines that include concrete trigger terms: domain words, subsystem names, common task names, or likely search phrases.
- If a lore doc answers a question the session still missed, improve the doc's filename, title, opening summary, or cross-links until the miss feels less likely next time.
- Promote only the rare, broadly relevant, easy-to-miss facts into `Important context`; keep the rest discoverable through the index.
- Avoid duplicate docs created only for discoverability. First try better names, better index text, and better links.

## Anti-patterns

- **Output every invocation.** If most reflect runs add or change something, the discipline is drifting — apply the scope filter, don't find something to say.
- **Docs that grow past a couple of pages.** Long docs bury the signal and accumulate stale claims. Split or trim.
- **Treating lore as a log.** Rewrite in place; lore is the current view, not annotated history.
- **Papering over retrieval failures with duplicate notes.** If the knowledge already exists, fix the path to it.
