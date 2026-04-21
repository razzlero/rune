---
name: retro
description: Retrospective on completed work — Keep/Problem/Try, maintains a lessons index
---

# retro

Reflect on a piece of work and record what's worth remembering.

Flow: the agent drafts observations first, the user adds theirs, then both converse about action items and key takeaways. The retro file is the working surface; `LESSONS.md` keeps durable takeaways quickly accessible.

## Retros dir

Write retros under `rune/retros/`. Create the dir if it doesn't exist.

## Drafting loop

1. **Pick a focus.** What work is the retro about — a recent plan (common), an incident, a phase of larger effort. Args, if provided, name or describe the focus. With no args and nothing capture-worthy in recent session, say so and exit.

2. **Create the file early.** Write `rune/retros/YYYY-MM-DD-<slug>.md` with the structure below. Seed each section (Keep / Problem / Try) with bullets of your own observations from the session or provided context — concrete and specific, not vague. Tell the user the path.

3. **Ask the user for their points.** "What would you add under Keep / Problem / Try?" The user's retro points matter at least as much as the agent's — don't skip this step. Update the file in-place as the user replies.

4. **Converse about action items and takeaways.** Which Problems warrant a fix? What's the specific change (skill edit, convention, doc update)? Which lessons generalize enough for `LESSONS.md`? Keep iterating on the file rather than holding answers in chat.

5. **Update LESSONS.md.** See the LESSONS.md section below for what to add.

6. **Finalize.** Report the final retro path and the bullets added to LESSONS.md.

## Retro file format

Path: `rune/retros/YYYY-MM-DD-<slug>.md`. Slug: kebab-case, 3–6 significant words naming the work being reflected on (feature, plan, phase). Collisions → append `-2`, `-3`.

Template: [reference/retro-template.md](reference/retro-template.md). Read before writing.

Bullets first. Expand into a paragraph only when the extra context is needed to make the observation actionable later.

## LESSONS.md — the running index

Path: `rune/retros/LESSONS.md`. A flat, scannable list of durable, generalizable lessons extracted from retros. For quick access — not a substitute for reading the source retro.

Template: [reference/lessons-template.md](reference/lessons-template.md).

When finalizing a retro:

- Add only **durable** lessons (true beyond today's environment) that are **generalizable** (likely to apply to future work). Skip one-off facts and incident narratives — those stay in the retro file.
- One line per lesson. If it needs a paragraph, it belongs in the retro, not the index.
- Group under the relevant scope heading. Create a new heading if needed.
- If a new lesson overlaps an existing entry, update the existing one instead of duplicating.

## Pruning outdated lessons

LESSONS.md is the live set of takeaways considered valuable right now; retro files are historical records of reflection. When a lesson no longer applies — the environment shifted, the skill changed, a newer retro contradicts it — just remove the bullet from LESSONS.md. Leave the source retro untouched; it's history, not doctrine.
