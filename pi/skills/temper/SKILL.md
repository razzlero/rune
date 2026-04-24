---
name: temper
description: Iterative fix-and-review loop on code changes until all models agree it's clean
---

# Temper

## Overview

Launch 3 fresh reviewer subagents in parallel across General / Fit / Stress lenses to review code changes against a plan/spec, hunting for both **issues** (bugs, contradictions, missing cases) and **improvements** (existing helpers to reuse, simpler patterns already in the codebase, refactor candidates). Address every actionable finding — fix, apply, or clarify — then restart with a fresh round. Converges when a round produces no actionable findings. Any applied change needs a fresh review pass — improvements can introduce bugs too.

For plan review (refining a plan/spec doc before implementation), use `/refine` instead — it's a single-pass comprehension audit with a different shape.

## Main session as judge

Reviewers work from the plan/spec document and the changed code. The main session has context they don't — the conversation, user intent, scope decisions, and the full picture of the work. That context advantage is what qualifies the main session to decide whether each finding is a real code issue (→ fix) or a misreading of the doc (→ clarify the doc). Reviewers surface candidates; the main session actively judges each one. Not a router mapping finding types to actions by rote — an arbiter using the fuller picture to pick the right action.

## Core Loop

3 reviewers per round, **one per lens**, launched in parallel as fresh subagents. Cross-model and cross-family agreement is the strongest convergence signal. Capping at 3 per round is an intentional cost/speed guardrail.

Three pre-defined multi-purpose lenses — General is itself a lens, so each round always runs a holistic reviewer alongside the focused ones:

| Lens | What it hunts |
|---|---|
| **General** | No narrow focus — reviews the change holistically. Catches cross-cutting issues, anomalies, and anything the focused lenses might miss as "not their lane" |
| **Fit** | Reuse and convention — existing helpers, patterns, or modules the change should use instead of inventing; new code conflicting with conventions visible in adjacent files |
| **Stress** | What breaks the change: edge cases (nil/empty/error paths, off-by-ones, happy-path skips), adversarial framing (shadow paths, ordering hazards), and scale / data-access (N+1 reads/writes, loops over DB or network calls, missing bulk/batch ops — mentally run at 10× and 1000× input) |

Use different models across the 3 slots when practical (e.g. haiku + sonnet + opus) for cross-model bias variation on top of lens diversity. Drop a focused lens entirely if clearly not applicable (e.g. Stress for a doc-only change) and use that slot for a second General reviewer instead.

1. **Launch all reviewers in parallel** — one fresh subagent per reviewer, so results come back as independent passes.
2. **Classify and act on every finding** — fix or clarify (see below). The main session judges which, using its context advantage. Severity: `critical` (bug, security, data loss), `important` (correctness, design), `suggestion` (improvement, simplification, style). Not majority vote — a finding raised by one reviewer is just as valid as one raised by all.
3. **Restart automatically** — if any changes were made (fix, applied improvement, or doc clarification), immediately launch a fresh parallel round from step 1 with new reviewer subagents (no context from prior rounds). Applied changes need their own clean pass — improvements and simplifications can introduce bugs too. Do not ask the user whether to continue — the loop runs until convergence.
4. **Done** — when a round produces no actionable findings, or only trivial feedback the main session judges not worth acting on. Convergence requires a clean pass that reviewed the *current* state of the code, not an earlier state. If the loop hasn't converged after 3 rounds, stop and report the remaining findings rather than looping indefinitely. Before declaring done, run the project's tests/builds/linters and read the output — don't assume fixes are correct from code inspection alone.

## How to Launch Reviewers

Each reviewer should start in a fresh session with no inherited chat context from the main session or prior rounds. Give each:

- The document path (plan/spec) and their assigned lens
- Instruction to read the plan first, then review the changed code (staged, uncommitted, or branch diff — whichever matches the workflow) **through their assigned lens** — other reviewers cover the other lenses. (General reviewers apply no narrow focus — that is their lens.)
- Instruction to skip issues already documented as known trade-offs — but still challenge documented decisions that conflict with conventions in adjacent code. A plan explaining a choice doesn't make it correct if it didn't name the convention it was deviating from
- Encouragement to look up related tickets, docs, or references mentioned in the plan, and to verify that any project documentation in the touched areas stays consistent with the change
- **Diminishing-returns reminder** — if the change is solid through their lens, say so. Don't manufacture findings to justify another round; trivial polish dilutes signal

Do NOT give any reviewer:

- Summary of what changed
- Context about prior review rounds
- List of files to look at

The reviewer must discover everything from the document and git state, like a real teammate would.

## Classifying Findings

| Finding type | Action |
|---|---|
| Real bug or correctness issue | Fix it |
| Valid edge case not handled | Fix or document as accepted trade-off |
| Issue already decided/documented | Update plan doc to be clearer |
| Docs-implementation mismatch | Fix whichever is wrong |
| Reuse opportunity or simplification | Apply if low-risk; note in plan if deferring |
| Stylistic preference | Note in plan as a convention/decision |

**Every finding gets one of two actions: fix or clarify.** No "skip" — every finding is evidence of either a real issue (→ fix) or a doc weakness that let the reviewer misread (→ clarify). The main session's context advantage picks between them.

If a reviewer's framing reads as "already decided", "stylistic", "informational", or "not applicable" — that's your signal the doc has a gap. Update it so a fresh reviewer wouldn't make the same call. Active judgment, not rote mapping from finding type to action.

**Convergence depends on this:** fresh reviewers have zero context; dismiss without updating the doc, and they'll raise the same point next round. The loop won't converge.

**Keep the doc dense, not long.** Fold clarifications into existing sections. Density over page count.

## Common Mistakes

- **Treating convergence as majority vote** — one reviewer finding an issue and two not is *not* convergence. Every finding needs action.
- **Skipping or routing passively** — no skip, but also no rote routing. Every finding deserves active judgment: real issue (→ fix) or reviewer misread (→ clarify).
- **Letting the plan bloat by appending** — each clarification folds into existing structure, not a new section.
- **Deferring to the plan when the codebase disagrees** — a plan's documented rationale doesn't override what reviewers see in adjacent code, especially if the rationale didn't name the convention it was deviating from.
