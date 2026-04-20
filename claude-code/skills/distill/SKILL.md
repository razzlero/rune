---
name: distill
description: Compact a doc to preserve meaning while cutting tokens and redundant instructions
---

# Distill

Compact prose and instructions. Keep meaning; cut tokens; collapse overlapping directives into fewer, stronger rules.

Use on AI-read or skim-read docs: plans, skill files, injected context. Not for READMEs, tutorials, or user-facing docs where tone matters. Not for code.

## Input

- **Target** — file path or inline text

## Core heuristic

**Cut the predictable, keep the surprising.** If a word, phrase, or whole rule is predictable from surrounding context, it's a safe cut. If it carries information the reader couldn't infer — a fact, a constraint, a reason, an edge case — keep it. Every rule below specializes this.

## Instruction economy

The biggest lever, and the one most often missed. Apply before prose polish; otherwise the real bloat survives and only filler words get trimmed.

- **Collapse echoed directives.** Same rule stated in multiple places weakens it rather than reinforcing it — restatement invites contradiction and muddies priority. Keep the strongest wording; delete the echoes. "Do X" + "Remember to X" + "Don't forget: X" → one line.
- **Cut examples subsumed by their principle.** If a general rule is stated and the examples just instantiate it without adding counterintuitive detail, drop the examples. Keep examples only when they pin down an edge case the principle doesn't spell out.
- **Don't explain what the reader already knows.** Drop definitions of common concepts (what a PDF is, what a migration is, what "idempotent" means). Challenge each explanatory clause: does the reader actually need this spelled out?
- **One term per concept.** Synonym drift ("API endpoint / URL / route / path" for the same thing) confuses. Pick one; replace the rest.
- **Options need a default.** Instead of "use X or Y or Z", write "use X (fallback: Y for case Z)". No undifferentiated option-lists in instructions.
- **Directive voice, not descriptive.** Skill files instruct an agent. Keep "You need a clean baseline to distinguish X from Y"; don't passivize to "a clean baseline is needed".
- **Restructure rather than compress, when possible.** Sometimes the right move is not "rewrite terser" but "move the detail to a linked file and leave a pointer". If a section can move to references without loss, prefer that over in-place compression.

## Prose economy

- Drop empty hedges: "basically", "really", "just", "simply", "actually", "essentially".
- Keep calibrated hedges ("probably", "generally", "usually", "often") when they scope a claim; drop when redundant. "This generally works" → keep "generally" (scopes the claim). "You should generally follow X" → drop the throat-clear, keep the directive.
- Drop restatement: "in other words", "that is to say", "what this means is" — and the repeat that follows.
- Drop throat-clearing: "note that", "keep in mind that", "it's worth mentioning that".
- Drop connective fluff that signals no real logical turn: "however", "furthermore", "additionally", "moreover". Keep when they mark a genuine contrast or addition.
- Canonical substitutions: "in order to" → "to", "utilize" → "use", "the reason is because" → "because", "make sure to" → drop (keep the verb), "it is important to" → drop (keep the verb), "a large number of" → "many".

## Merge and restructure

- Merge adjacent bullets making the same point. Fold three-sentence paragraphs into one when wording permits. Reorder so related points sit together and cross-references drop out.
- Two sections genuinely duplicate content? Merge under one heading — but preserve navigational structure (see below).
- When multiple examples illustrate the same pattern, keep the simplest canonical one; drop the rest.
- Preserve *navigational* structure — top-level headings a reader relies on to find things. Minor structure (bullet vs prose, section order within a group, how sub-points nest) is fair game.

## Preserve even when it looks like filler

Main failure mode: cutting phrases that look like filler but carry meaning. Before deleting, confirm meaning survives:

- **Truth-condition qualifiers** — "always", "only", "never", "except when X", "unless Y". Cutting silently flips what's true vs false. Highest-stakes category.
- **Negatives** — "don't", "not", "no", "never". Easy to drop under rewriting and invert meaning. Double-check any rewrite that removes a negation.
- **Defining or narrowing parentheticals** — parentheticals that scope or define the word before them. "If the test itself is wrong (not testing what you intended), fix it" — without the parenthetical, "wrong" could mean failing when it should pass. Filler parentheticals ("(by the way, ...)") are still fair game.
- **Scope anchors** — short phrases anchoring *what* a claim applies to: "in this file", "for this step", "within the current module". Dropping widens the claim.
- **Reasons attached to rules** — a rule's "why" governs how the reader applies it at edges. "Keep tests hermetic (so parallel runs don't flake)" — cutting the parenthetical keeps the imperative but loses the criterion for judging new cases.
- **Retrieval-critical text** — text an agent matches on to decide *whether to invoke something*: skill `description` frontmatter, agent `description` frontmatter, skill trigger examples, the first sentence of a plan, any session-primer text injected into every conversation. These drive activation; paraphrasing changes behavior upstream of the reader. Compress minimally, if at all. *Body* content is normal material.
- **Principle and role framing** — prose that explains *who* has authority and *why* a step matters ("the main session judges each finding", "reviewers surface candidates; the outer session decides"). Usually multi-sentence paragraphs that carry a skill's *spirit*. Compressing into absolute imperatives ("no skip", "every X must Y") preserves the letter but strips *who* and *why* — the reader loses the judgment cue and the skill reads as procedure-to-follow instead of judgment-to-apply. Keep as prose.
- **Grammatical integrity** — don't drop objects of verbs when the antecedent isn't obvious from the same clause. "The skill will detect and stop" leaves "detect" dangling; "the skill will detect this and stop" is clear.

When in doubt, keep. Longer sentence > ambiguous short one.

## Do not compress

- **Exact error messages, log strings, command output** — quote verbatim. Paraphrasing breaks grep and misleads debugging.
- **Destructive or irreversible action warnings** — "this deletes all records" needs full prose. Stakes must survive.
- **Security-sensitive steps** — authentication flows, permission checks, credential handling. Clarity > density.
- **Ordered sequences where fragment-order is ambiguous** — if the compressed version could be misread as a different order, keep the connectives.
- **Code, inline code, URLs, paths, identifiers** — never altered. Table *structure* (headers, column count) preserved; table cells contain prose and are fair game.

## Process

1. Read target. Whole file if file; provided text if inline. If no target was provided, report it and stop — don't guess at intent.
2. Mark protected spans: code, URLs, paths, identifiers, "do not compress" regions, retrieval-critical text. These pass through with only trivial filler drops.
3. **Instruction-economy pass** — look for echoed directives, principle+example pairs where the examples add nothing, synonym drift, undefaulted option-lists, unnecessary explanations, descriptive voice where directive is right. This is where the biggest wins come from; skipping it leaves the real bloat intact.
4. **Prose-economy pass** — apply drop rules section by section (not whole doc at once — easier to stay on track).
5. Verify meaning preserved. Read distilled vs original. Every fact, decision, reference, negation, and truth-condition qualifier must survive. Rule produced ambiguity? Back off that sentence.
6. Output:
   - File target → overwrite in place. Git is the undo path. If the file is dirty (uncommitted changes), surface that to the caller before overwriting; don't silently stomp in-progress work.
   - Inline target → return the distilled text in the reply.
   - If the caller explicitly asked for a new file, write alongside the original with a `.distilled.md` suffix.

## Self-check

- No code, URL, or identifier altered.
- No fact, number, or name dropped.
- No negation or truth-condition qualifier lost or moved.
- "Do not compress" regions reached the output intact.
- Retrieval-critical text is near-verbatim.
- Navigational structure matches original (top-level headings intact).
- Echoed directives collapsed, not just shortened individually — instruction count dropped, not only word count.
- Reads as full prose; no telegraphic shorthand.

## Common mistakes

- Touching code or identifiers — distill is prose-only.
- Cutting tokens without cutting *instructions* — the bigger lever is redundant directives, not filler words. A one-line rule plus three bullets echoing it should become one line.
- Dropping a rule's "why" while keeping the imperative — the reader loses the criterion for judging edge cases and misapplies the rule.
- Flipping truth conditions — dropping "always", "only", "never", or "except when X" silently changes what's true.
- Dropping or moving a negation — inverts meaning. Highest-severity rewriting error.
- Cutting defining parentheticals as filler — scope/definition parentheticals aren't optional.
- Dropping scope anchors — "in this file", "for this step" widen the claim when removed.
- Turning directives into descriptions — passive voice weakens skill instructions.
- Flattening principle into procedure — collapsing role/authority framing into absolute imperatives preserves the letter, strips the spirit.
- Paraphrasing retrieval-critical text — descriptions, trigger examples, and primer text drive activation upstream of the reader; paraphrasing changes behavior.
- Restructuring into a different document — merging bullets and folding paragraphs is fine; rearranging top-level navigation isn't. A reader who knew where things lived should still find them.
