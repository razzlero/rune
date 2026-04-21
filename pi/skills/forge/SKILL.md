---
name: forge
description: Execute a plan with TDD discipline — red, green, refactor
---

# Forge

## Overview

Follow a plan and turn it into working change. Default to TDD — write the failing test first, make it pass with minimum code, then refactor. Keep plan, docs, and code in sync as implementation reveals gaps. Verify before claiming done — run tests/builds/linters and read the output.

## Input

A plan to implement — from `/craft`, a structured task list, or any description of work with identifiable steps. Default to the most recently discussed plan in the conversation. If multiple plans exist or the reference is ambiguous, ask before proceeding. If no plan can be identified, ask for one or suggest running `/craft` first.

If the plan is a file, read it at the start. If conversational, work from conversation context.

## Before Starting

### Create tasks from plan steps

Create a task for each plan step before writing any code using whatever task-tracking surface is available in pi or in your repo. Mark each in-progress when starting, completed when done.

### Survey the codebase

Before the first step:

1. **Existing patterns** — skim adjacent code the plan touches. Note naming, structure, and conventions so new code fits in.
2. **Test infrastructure** — look for test files (`*.test.*`, `*.spec.*`, `*_test.*`, `tests/`), configs (jest.config, pytest.ini, vitest.config, phpunit.xml), and test commands (package.json scripts, Makefile). Note the framework and how to run tests.
3. **Baseline** — if tests exist, run the suite. A clean baseline lets you distinguish pre-existing failures from regressions you introduce. If the baseline is red, report before proceeding.

If no test infrastructure exists and the work is genuinely behavioral code that would benefit from tests, surface that to the user before forging ahead — don't silently skip testing or set up a test framework on your own initiative.

## Process

Work through the plan step by step. For each step: red, green, refactor.

**Red — failing test first.**

1. Write a test that asserts the behavior the step requires. Follow the codebase's existing test patterns for naming, structure, and location.
2. Run it and confirm it fails _for the right reason_ — missing behavior, not an import error, typo, or setup glitch.
3. If the test unexpectedly passes, investigate: behavior may already exist (check for existing coverage; delete the duplicate test if so, or keep as additive coverage if not). If the test is wrong, fix it.

Keep tests focused. One test per behavior, not one per function.

**Green — minimum code to pass.**

1. Write the simplest real implementation that makes the test pass. Not hardcoded returns; not code you'll immediately replace.
2. Run the test and confirm it passes.

**Refactor — only if it helps.**

1. With the test green, look for duplication, naming, or structural improvements.
2. Follow existing patterns in the codebase — consistency with neighbors matters more than a locally "better" choice. One repeated pattern is easier to read than competing ones.
3. Re-run the test. Skip refactoring if the code is already clean.

**Escape clause.** If a step has no behavior to assert (prompt authoring, config, docs, infra glue), skip the test and implement directly — verify by whatever fits (manual run, visual check, lint, type check). The bar is genuinely no meaningful behavior, not "writing a test feels awkward" — awkward tests usually mean the design needs adjusting.

### Between steps

- Run the affected module's tests every few steps; run the full suite less often but at least before reporting completion.
- If a regression appears, stop and fix it before continuing.
- If implementation reveals the plan is wrong, update the plan (or state the deviation if conversational) before moving on. Don't silently drift.

## Guidelines

- **Follow the plan order.** Steps may have dependencies. If a step doesn't make sense yet, check if you missed something earlier.
- **Match the codebase's style.** Test style, naming, module layout, error handling — consistency with existing patterns applies everywhere.
- **Don't test implementation details.** Test behavior and outcomes. Tests on private methods or internal state become brittle.
- **Distinguish test failures from runner failures.** Missing deps, compilation errors in unrelated code, and environment issues are infrastructure problems — report them rather than working around them.
- **Watch for skipped tests.** Confirm tests actually ran and produced pass/fail — skip/pending/todo markers can exit cleanly without running anything.
- **Keep docs in sync.** If behavior covered by READMEs, API docs, or architecture docs changes, update the docs in the same pass.
- **Stop when blocked.** If a step can't be completed — unclear requirement, missing dependency, plan assumption that doesn't match reality — stop and ask. A wrong guess compounds.

## Completion

When all plan steps are done:

1. Run the full test suite and confirm it passes.
2. Report: steps completed and any plan deviations.
3. Ask the user whether to run `/temper` in code review mode as a final quality check.
