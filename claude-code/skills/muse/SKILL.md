---
name: muse
description: Capture raw thoughts into an organized musing doc — pre-planning scratchpad and thinking partner
---

# muse

Pre-planning scratchpad and thinking partner. Produces a durable musing document under `rune/musings/` that covers three overlapping uses — problem/ticket exploration, rubber-duck partnership, and thought capture — as one flow with three postures, not three modes.

Muse is an **optional** upstream step for `/craft`. Most changes go straight to craft. Reach for muse when the starting point is too fuzzy for a plan: unclear problem, competing options, unformed ideas. A musing can feed into a later `/craft`, or stand alone — "I organized my thinking" is a valid terminal state.

## Musings dir

Write musings under `rune/musings/`. Create the dir if it doesn't exist.

## Invocation modes — three peer forms, not fallbacks

| Invocation | Primary source |
|---|---|
| `/muse` (no args) | Recent session — capture or continue thinking from the current session context. |
| `/muse <short framing or ticket ref>` | Short arg biases the topic. A ticket reference or problem statement triggers proactive resource gathering (see Drafting loop § Gather). |
| `/muse <pasted context block>` | Long arg provides raw material to organize. No recent session required. |

Distinguish short framing from a context block by shape — a terse phrase is framing; a paragraph is context. If ambiguous, ask once rather than guessing.

## Postures

The three uses are postures on a spectrum of "how much does the user bring vs. how much does muse contribute." A session can slide between them:

- **Exploration** — a ticket or problem statement invites active investigation. Muse searches the codebase, fetches linked tickets/docs, web-searches external references, surfaces options, flags unknowns.
- **Rubber-duck** — a "what do you think about…" invites reflection. Muse responds, challenges weak spots, proposes alternatives, captures the exchange.
- **Capture** — a paste invites organization. Muse sorts into sections without contributing much of its own.

Read the user's input signal to pick a starting posture; adjust as the session evolves. Do not declare a mode.

## Drafting loop

1. **Sketch a stub.** Pick an initial slug and title. Write `rune/musings/YYYY-MM-DD-<slug>.md` with `# <title>` and any sections the input already justifies (drop the rest). Tell the user the path — this file is the working surface.

2. **Gather** — if the input is a ticket/problem, proactively pull in relevant resources: read referenced tickets with whatever MCP tools are available this session, grep the codebase for obvious entry points, web-search external references the problem names, skim linked docs. Record findings under `Context` or `Resources` — raw where it can't be re-derived, summarized otherwise. Skip gathering for pure idea-dumping input.

3. **Diverge.** In exploration or rubber-duck posture, generate broadly before pruning. Populate `Ideas` and `Options` with multiple framings. In rubber-duck posture, offer critique inline — challenge weak spots, suggest alternatives, flag assumptions. Muse's value is *widening* the space; jumping straight to a recommendation defeats the purpose (`/craft` is the right tool for narrowing).

4. **Ask focused questions** — one at a time, when clarification or a fork genuinely needs user input. Not a rapid-fire interview. Update the file in-place as answers land. Re-read the file before each meaningful edit so you're extending, not contradicting, prior content.

5. **Curate.** As understanding firms up:
   - Rewrite loose bullets into tighter ones.
   - Move resolved questions out of `Open questions`.
   - Drop ideas the user has ruled out.
   - **Distill staged raw material.** For each raw block, pick one: (a) keep verbatim if a specific fragment carries value later (exact field names, quoted constraints, irreplaceable phrasings); (b) summarize into prose and delete the raw; (c) collapse to a one-liner — "Early framing was X; superseded by Y" — when the content shaped the thinking but isn't worth quoting in full.

   Actively edit; don't append. The file must stay skimmable — if you can't re-read it in full before a decision, it's too long. The file is a knowledge base, not a chronological session log.

6. **Converge** — when the user signals direction or options stop multiplying, start populating `Leaning`. "No decision yet" is a valid stopping point; muse should not force convergence.

7. **Stop when useful, not when finished.** Finalize when the user says so, or when further input isn't available. Note if the material feels plan-ready, but stop there — do not invoke or auto-progress into `/craft`. Rune's skills hand off manually; the user picks the next step.

## Muse file format

Path: `rune/musings/YYYY-MM-DD-<slug>.md`

Slug: kebab-case, 3-6 significant words describing the topic. Collisions → append `-2`, `-3`, etc.

Sections are optional and the order can vary — the shape should serve the material, not force it. Only the title and framing are required.

```markdown
# <short title>

<one or two sentences: what this muse is about and what triggered it>

## Context
<problem statement, ticket excerpts, quoted user thoughts — raw material
preserved verbatim when it can't be re-derived, summarized otherwise.>

## Resources
<external links, related docs, referenced tickets. Add a one-line summary
next to each so the file stays useful without following every link.>

## Ideas
<raw thoughts, observations, possibilities. Bullets. Contradictions allowed
during divergence.>

## Options
<specific approaches being weighed. For each: what it is, what it costs,
what it buys.>

## Critique
<muse's pushback on the user's framing — weak spots, missing considerations,
alternative angles. Fold into `Options` when critique is option-shaped.>

## Open questions
<what's unknown or unresolved. Questions for the user, gaps in understanding,
things to investigate.>

## Leaning
<current direction, if any. May be "none yet". Short — a pointer toward
where the thinking has landed.>
```

Drop any section that doesn't apply. Add subsections when one grows.

## Handoff to craft

When the material feels plan-ready, note that in the muse (e.g., a line at the end of `Leaning` or a brief wrap-up) and stop. The user chooses whether and when to run `/craft <muse path>`. Muse must not invoke craft or auto-progress into it — rune's skills hand off manually, each stopping at its boundary.

When the user later runs `/craft`, the muse stays on disk as historical record and is linked from the plan's Overview → References. It isn't consumed or moved.

## What muse does NOT do

- No implementation, no code changes.
- No forcing convergence — "no decision yet" is a valid outcome.
- No plan-shaped output — if the material is ready for a plan, the user runs `/craft`.
- No silent transcription in rubber-duck flow — critique and reflection are part of the job.
- No chronological session-log shape — the file is curated, not appended.
- No auto-invoking or auto-suggesting the next skill — manual transitions only.
