---
name: craft
description: Craft a plan that captures everything an implementer needs to hit the ground running
---

# Craft

## Overview

Explore a problem space and produce a durable plan document covering one unit of work. The document is a compaction of the planning conversation — if the conversation were lost, the document alone should be sufficient for an implementer to continue.

Plans, docs, and implementation should stay in sync; flag affected docs so the implementer can update them.

## File Location

Plans go under `rune/plans/`. Create the dir if it doesn't exist.

Prefix each file with the current date: `rune/plans/YYYY-MM-DD-<topic>.md`.

## Planning Process

### 1. Explore — write the file as you go

The plan file is working memory, not a deliverable produced at the end. Conversation context is lossy and disappears on compaction — findings, questions, and proposed approaches land in the file _as they emerge_, even in rough form.

**Create the file at the start of investigation**, not after alignment. Use the stub format from [Document format](#document-format); sections start sparse and fill in as you learn. Anything you'd otherwise raise in chat goes into Open Questions.

**Write notes as you investigate.** Findings land in Overview (intent/scope), Assumptions (bets you're making), Notes (observed context), or Open Questions (direction-blockers). Notes is the default when placement isn't obvious. Raw is fine — condense as understanding solidifies.

- **Write promptly** — before findings fall out of context.
- **Prioritize external context that can't be re-derived** — ticket content, API responses, Slack discussions, error messages.
- **Transcribe perishable visuals** — screenshots, diagrams, browser results — before moving on.
- **Capture code structure, not a full re-walk** — key file paths, patterns, interfaces. Code can be re-read; re-exploring is expensive.
- **Re-read the file before major decisions** — keeps goals and prior findings in the attention window.

**Ask rather than fill gaps.** If an Overview bullet isn't grounded in user statements, referenced tickets, or the conversation, raise it as a question rather than writing it. Uncontested Overview bullets read like recorded intent but are invented by the agent. Prefer one focused question at a time; batch 2-3 only when independent and genuinely blocking.

**Assess scope early** — if the work spans multiple subsystems or has loosely related parts, flag it. The plan may need to narrow before it's useful.

**Get alignment against the plan** — point the user at the in-progress file rather than re-summarizing in chat. Resolve questions by noting the answer alongside the question in Open Questions. Omit the section when empty.

### 2. Assess the current state

Before finalizing, understand what exists:

- **Code:** Read the relevant code areas to understand current behavior and patterns. Pay attention to conventions — how are similar types, interfaces, and patterns handled elsewhere? These conventions are constraints on the design choices.
- **Docs:** Find docs relevant to the area you're changing and verify them against the code (docs drift). Fix small gaps directly; flag substantially outdated docs as a pre-implementation step. Only update docs to reflect the _current_ state — target-state doc changes are the implementer's job. Accurate baseline docs let the plan reference them instead of re-describing the system.
- **Prior planning history:** Are there prior plans in the repo? Read recent ones for context and conventions.

### 3. Write the plan

Concrete details — file paths, links to examples, short snippets showing a pattern to follow — earn their space when they save the implementer from re-investigating. Enough for the implementer to start with confidence; the actual changes are their judgment.

**Don't name helpers or wrappers in Steps unless the abstraction is load-bearing.** Naming `capXForY` primes the implementer to ship ceremony; "cap X at N graphemes via `existingUtil`" is enough.

## Document format

Read [reference/plan-template.md](reference/plan-template.md) before writing the file. Each section builds on the ones above it, so the reader accumulates context as they go.

## Design principles

1. **The document replaces the conversation.** If the planning session were lost, could an implementer start from the document alone? External context (tickets, API behavior, discussions) must be captured — it's gone otherwise. Code context (key files, patterns, interfaces) should be captured enough to avoid a full re-exploration, but not so much that the document bloats.
2. **Facts, not invented rationale.** Rationale must trace to something the implementer can also see — the conversation, the code, or docs they'd also rely on. Private scratchpads and musings don't count. Don't write _"X because Y"_ for choices the user didn't debate; plausible-sounding reasons read the same as recorded context and mislead. If a reason feels useful but isn't grounded, omit it, label it as an assumption, or raise it as an open question.
3. **No duplication across sections.** Verification criteria live only in the Verification section; the final Implementation Step points at it, never inlines it. Non-Goals live only under Overview. Assumptions are bets (could be wrong); Notes are observed facts — the same statement doesn't go in both.
4. **Dense, not long.** Fold new content into existing structure, don't append. Grow density, not page count.
   - One idea per bullet; collapse to one sentence.
   - Max two nesting levels.
   - Drop hedges ("basically", "just"), restatement, throat-clearing.
   - Alternate bullets and prose — all-bullets flattens emphasis.
   - Keep negations, qualifiers ("only", "never"), scope anchors, file paths.
5. **New patterns follow existing conventions.** When the plan introduces a new type, interface, or pattern, it must match how the codebase already handles similar things. A locally reasonable choice that contradicts an established convention is still wrong unless there's a strong reason to deviate — and the plan must name the convention it's deviating from and explain why. "It's simpler this way" is not sufficient; "it deviates from the X convention because Y" is.

**Don't include as steps:**

- **`/retro`** — retros are for capturing problems-and-fixes after the fact (manual, user-driven). Not an implementation deliverable.
- **Git commits** — committing is the user's manual concern, outside AI flows. Don't add a "commit the changes" step unless the user explicitly asks.
- **"Write tests" as a trailing step** — tests belong with the feature step that introduces the behavior, not appended after. A final "review test coverage" step that confirms what was built has appropriate coverage is fine; a catch-up "now write the tests" step is not — it reads as permission to defer testing.

### 4. Self-check before review

Before submitting for review, verify:

- **Ambiguity scan** — would two developers interpret any section differently? Tighten it.
- **Rationale audit** — for each _"because"_ or _"why"_ in the plan, confirm the reason is traceable (conversation, code, docs the implementer would also rely on). Untraceable = invented — remove, mark as assumption, or raise as open question. Mental check, not inline annotation.
- **Reference audit** — verify every cited file path, function, type, or helper against the code. Invented ones read identical to real ones. One grep per non-obvious claim.
- **Overview audit** — every Overview claim (prose lead-in, Goals bullet, Non-Goal) must trace to user input, a linked reference, or a confirmed clarifying-question resolution. Ungrounded claims must be raised or removed.
- **Scope check** — did the plan grow beyond the original goal? Trim or split.
- **Convention consistency** — for every new type, interface, or pattern, verify it matches how the codebase handles similar things. If it doesn't, either change the design or name the convention and justify the deviation inline.
- **Doc consistency** — does the plan contradict any existing docs or the stated goal?
- **Completeness** — could a developer start work without clarifying questions? Could someone resume from this plan after a full context reset?
- **No unresolved placeholders** — every step should be concrete enough to implement. Stray "TBD" or "TODO" means investigation isn't finished.

### 5. Hand off

Plan is done. Stop here and let the user pick the next step: `/refine` to stress-test the plan from the no-context implementer's perspective, or `/forge` to execute. If Open Questions remain, flag them to the user rather than handing off.
