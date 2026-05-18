---
name: distill
description: Compact a doc to preserve meaning while cutting tokens and redundant instructions
---

# Distill

Compact AI-read working docs so a future session keeps the same useful state with fewer tokens. Rewrite as **state transfer**, not summary: preserve what the next agent needs to continue without the original conversation.

Use on working docs where token pressure matters. Not for code, tutorials, marketing copy, or docs where voice matters more than compression.

## Core model

Use the same bias good context compaction uses:

- **Keep durable state; drop the path taken to reach it.** Preserve decisions, constraints, current status, open questions, and next actions. Drop dead conversational scaffolding.
- **Cut the predictable; keep the surprising.** If the reader could infer it from nearby text, cut it. Keep facts, exceptions, rationale that changes edge-case handling, and non-obvious relationships.
- **Front-load high-value state.** Important context is easiest to miss when buried in the middle. Move critical constraints, decisions, and next steps toward the top of the doc or section.
- **Prefer one strong rule over repeated reminders.** Redundant instructions waste tokens and weaken priority.
- **Optimize for retrieval, not only compression.** A shorter doc that hides the key fact is worse. Use concrete headings and trigger terms so a future agent can find the right section fast.

## Preserve first

Before cutting, identify the information a future session would regret losing:

- current objective or document purpose
- decisions made and why they still hold
- invariants, constraints, and truth-condition qualifiers (`only`, `never`, `unless`, `except`)
- open questions, risks, and unresolved tradeoffs
- next actions or handoff guidance
- exact strings needed for retrieval or execution: code, identifiers, paths, URLs, commands, error text
- retrieval-critical text: skill `description`, trigger examples, opening lines that determine whether the doc gets opened at all

## What to cut hard

- throat-clearing, filler, and obvious connective prose
- repeated restatements of the same rule
- examples that add no edge case beyond the rule they illustrate
- long option lists without a default
- explanations of common concepts the reader already knows
- historical play-by-play when the resulting state is already captured
- failed approaches that do not explain a current constraint, warning, or decision

## Compaction moves

### 1. Rewrite transcript into state

Turn:
- what was discussed
- what was tried in sequence
- who said what

Into:
- current status
- settled decisions
- active constraints
- remaining questions
- next steps

If the reader only needs the result, cut the narrative.

### 2. Collapse instruction count

Merge overlapping directives. Replace:
- a principle
- two reminders
- three examples saying the same thing

with one rule plus one example only if the example carries non-obvious edge meaning.

### 3. Promote durable facts; demote local detail

Keep architecture, conventions, mappings, and durable gotchas. Cut local implementation trivia unless it affects the next decision.

### 4. Reorder by operational value

Within a doc or section, prefer this rough order when it fits:
1. purpose / current status
2. must-know constraints and decisions
3. details needed to act correctly
4. references and appendices

Do not preserve a weak order just because it was original.

### 5. Sharpen retrieval cues

If a section is easy to miss, improve the heading or opening line. Prefer names that advertise both topic and use-case: what this section covers, and when to read it.

## Meaning-preservation traps

Do not cut these just because they look compressible:

- negations (`not`, `never`, `don't`)
- qualifiers (`always`, `only`, `unless`, `except when`)
- defining parentheticals that narrow meaning
- scope anchors (`in this file`, `for this step`, `within this repo`)
- reasons that control edge-case judgment
- role framing that explains who decides or why a rule exists

When in doubt, keep the longer sentence.

## Do not compress

- code, inline code, identifiers, paths, URLs, commands
- exact error messages or log text
- destructive-action warnings
- security-sensitive instructions
- ordered procedures where shortening could blur sequence

## Process

1. Read the document.
2. Mark protected spans: code, identifiers, URLs, paths, exact strings, and any do-not-compress regions.
3. Identify the doc's durable state: purpose, constraints, decisions, open questions, next steps.
4. Rewrite around that state. Remove transcript-like history unless it still carries active meaning.
5. Compress prose and collapse repeated directives.
6. Reorder for findability when helpful; front-load critical information.
7. Verify that a future reader could act correctly without the original text.
8. Output:
   - File target → overwrite in place. Git is the undo path. If the file is dirty, surface that before overwriting.
   - Inline target → return the distilled text in the reply.
   - If the caller explicitly asked for a new file, write alongside the original with a `.distilled.md` suffix.

## Self-check

- The distilled doc preserves current state, not just topic coverage.
- A future agent could continue the work without the original conversation.
- No code, identifier, path, URL, command, or exact error text changed.
- No negation, qualifier, constraint, or decision rationale was lost.
- Repeated directives were merged, not merely shortened one by one.
- Important facts were moved earlier when they were previously buried.
- Headings and opening lines still help retrieval.
- The result reads as clear prose, not telegraphic fragments.

## Common mistakes

- Producing a summary instead of a handoff document.
- Keeping chronological history when only the resulting state matters.
- Shortening prose without reducing instruction count.
- Dropping the reason that tells the reader how to apply a rule at the edges.
- Burying the key constraint in the middle of a shorter doc.
- Making text denser but harder to retrieve.
- Touching code or exact strings.
