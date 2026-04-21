```markdown
# [Title]

## Overview
One or two sentences: what this plan is about and its high-level intent. A reader should know within 10 seconds whether it's relevant. The Overview is the plan's anchor; every section below elaborates on what's stated here.

### Goals
- Expected outcomes — intent-level, not technical. Bullets that would survive a complete redesign of the technical approach.

### Non-Goals
- What this plan explicitly does NOT aim for. Can reference technical areas when the intent is "stay out of this subsystem."

### References
Links that source the intent — tickets, musings, product decisions, discussions. One line each with a short label. Omit the subsection if empty.

## Assumptions
Beliefs the plan depends on that could turn out wrong — the first things to sanity-check if the work stalls. Distinct from Notes (observed facts): Assumptions are bets. Omit if empty.

## Notes
Preserved external context that can't be re-derived — ticket details, API behavior, error messages, discussion outcomes, quirks. Inline links where a note cites a source. Bullets by default, one idea per bullet. Condensed from raw investigation; not a scratchpad.

## Open Questions
Known unknowns to resolve before implementation can start. Note each answer alongside its question. Omit the section if empty.

- [ ] Question 1
- [ ] ...

## Implementation Steps
Numbered steps describing what to achieve, not how. Include file paths and short snippets where they help; don't pre-write the implementation. Flag docs that should be updated in a step. Link external references inline where relevant.

**The final step is always:** *"Verify against the Verification checklist."* A one-line pointer so forge tracks the audit as a task.

## Verification
Audit checklist against the Overview. GitHub-style checkboxes so each criterion can be ticked off as it's confirmed. Executed as the final Implementation Step.

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] ...
```
