---
name: refine
description: Refine a plan by stress-testing it from the no-context implementer's perspective
---

# Refine

## Overview

A plan is only as good as the worker who'll execute it can follow. `/refine` launches no-context reviewer subagents — proxies for the implementer who'll inherit the plan — and surfaces what's unclear, ambiguous, or contradictory before the work starts.

Single pass, parallel reviewers, main session triages and folds clarifications back into the plan. No convergence loop — plan review is a comprehension audit, not a bug hunt; one well-structured pass with diverse lenses beats N rounds of polish.

## Input

A plan document path. If none is given, default to the most recently modified file under `rune/plans/` and confirm with the user before proceeding.

## Process

### Phase 1: Launch reviewers in parallel

3 reviewers per pass, **one per lens**, launched in a single message with multiple Agent tool calls (do NOT use `run_in_background` — foreground batch only, so results return together). Capping at 3 is an intentional cost/speed guardrail.

Three pre-defined multi-purpose lenses — General is itself a lens, so every pass runs a holistic reviewer alongside the focused ones:

| Lens | What it catches |
|---|---|
| **General** | No narrow focus — reads the plan holistically. Catches cross-cutting gaps, contradictions between sections, and anything the focused lenses might miss as "not their lane" |
| **Implementer** | The worker's perspective throughout the task: what's confusing at hour 1 (cold-open clarity), what surprises hit mid-implementation (predictability from the plan), and what behaviors, edge cases, or success criteria are left unspecified (test-author concerns) |
| **Operations** | First on-call paged at 3am — failure modes, observability, rollback. What does the plan say (or fail to say) about what breaks and how to recover? |

Use different models across the 3 slots (e.g. haiku + sonnet + opus) for cross-model bias variation on top of lens diversity. Drop a focused lens entirely if clearly not applicable (e.g. Operations for a pure refactor) and use that slot for a second General reviewer instead.

Give each reviewer:

- The plan path
- Their assigned lens (verbatim from the table above) and instruction to review *only* through that lens. (General reviewers apply no narrow focus — that is their lens.)
- Instruction to read related documents/tickets/references mentioned in the plan, and to read adjacent code in the areas the plan touches
- The output shape (next section), with a hard cap of ~5 items per section
- An instruction to **not** paraphrase, summarize, list strengths, or rate severity — those are noise for plan review

Do NOT give any reviewer:

- A summary of the plan
- Context from prior reviews
- Hints about what other lenses are looking for

### Phase 2: Reviewer output shape

Each reviewer returns three short sections, each capped at ~5 items:

- **Questions** — questions the worker would need answered before or during the work. Specific, not vague. *"What does X mean here?"* / *"Where should Y live?"* / *"What should happen when Z?"* — unknown-unknowns surface as questions, cheaply.
- **Predictions** — what the reviewer expects to hit that the plan doesn't prepare them for. Load-bearing for Implementer and Operations; optional for General.
- **Issues** — contradictions in the plan, factual errors, conflicts with the codebase or with referenced docs. Single line each.

No paraphrase, no summary-back, no ratings. The reviewer's *questions* are the comprehension signal — if they had to ask, the plan didn't make it findable.

### Phase 3: Main session triages

Every item from every reviewer gets one of two actions. There is no skip.

| Item type | Action |
|---|---|
| Question or prediction the worker would actually hit | Answer it from the plan, the ticket, linked docs, the conversation, or adjacent code — then fold the answer into an existing section. If genuinely unanswerable from those sources, add to the unresolved list for Phase 4. |
| Question already answered in the plan | The answer isn't where the reviewer looked. Move it, restructure, or add a cross-reference so a fresh reviewer would find it. |
| Issue (contradiction, error, codebase conflict) | Fix the plan — or, if the reviewer misread, clarify the doc so a fresh reviewer wouldn't make the same call. |

If a question reads as *"obvious to anyone with context"* — that **is** the gap. The reviewer is the proxy worker; their context is what the plan provides. The main session's context advantage is for *answering* questions, not dismissing them; no item gets dismissed without a doc change.

**Answer from context first; surface as a last resort.** Silent invention — folding in a plausible-sounding answer the outer session actually doesn't know — is the main failure mode. When genuinely unsure whether you can answer from available sources, put the item on the unresolved list. A cheap user round-trip beats a confidently-wrong plan edit.

**Density over page count.** Fold clarifications into existing structure. Do not append a Q&A section, do not add a "Clarifications" appendix. If the plan's Overview, Strategy, or Implementation Steps weren't clear enough, those are what gets edited.

### Phase 4: Surface unresolved items

If the unresolved list is empty, skip to Phase 5.

Otherwise, present the whole list to the user as **one batched message** — grouped questions, no per-item round-trips. Wait for the user's answers, then fold them into the plan the same way as Phase 3 resolutions. If new uncertainties surface from the user's answers, that's a signal for another `/refine` pass, not another round of pinging.

### Phase 5: Hand off

The plan is revised. Stop here — the user picks the next step (typically `/forge` to execute, or another `/refine` pass if revisions were substantial enough that a fresh reviewer might see something new).

## Common mistakes

- **Letting reviewers paraphrase the plan** — token-heavy, low-signal. The output shape is questions / predictions / issues, not a recap. Cap each section.
- **Treating reviewer questions as "they didn't read carefully"** — the reviewer *is* the no-context worker. If they had to ask, the plan didn't make it findable.
- **Appending a Q&A section instead of revising** — clarifications go *into* existing sections. Density over page count.
- **Dropping items because they feel obvious** — obvious-to-you is the gap; obvious-to-the-reader is the goal. Every item gets fold-or-restructure.
- **Running multiple rounds by reflex** — single pass is the default. Re-run only when revisions warrant it.
- **Silent invention under uncertainty** — folding in a plausible-sounding answer the outer session doesn't actually know puts a confidently-wrong edit into the plan. Answer from the plan/ticket/linked docs/adjacent code first; what survives goes on the unresolved list.
- **Pinging the user per item** — Phase 4 is one batched message, not N round-trips. Batching is the confirmation-spam defense.
