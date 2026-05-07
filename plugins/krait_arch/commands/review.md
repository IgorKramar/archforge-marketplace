---
description: Phase 5 of the Architecture Cycle — architectural code review of changed files or a path.
argument-hint: "[path or git ref] (optional; defaults to staged + last commit)"
---

# /krait_arch:review

Phase 5 of the Architecture Cycle. Architectural review of code, with the project's ADRs and `ARCHITECTURE.md` as binding context.

## Inputs

- Target: `$ARGUMENTS` if provided. Interpret as either:
  - A file path (review that file/directory).
  - A git ref or PR description (review the changes in that ref).
  - Empty → review staged changes plus the last commit on the current branch.
- Project context: `./ARCHITECTURE.md`, all ADRs in `./docs/architecture/decisions/`.

## What to produce

Use the `code-review-architectural` skill. Output structure (mandatory):

```
## Summary
2–3 sentences: what the changes are doing and the main architectural risk or concern.

## Conformance with ADRs
For each relevant ADR (by number), state whether the change:
- Conforms.
- Conflicts (and how — this is a blocker unless a new ADR supersedes).
- Is silent (the ADR doesn't speak to this change).

## Blocking issues
Things that should be fixed before merge. For each:
- Pointer to specific code location.
- Why it's a blocker (architectural reason, not stylistic).
- Suggested fix.

## Non-blocking suggestions
"nit:" or "suggestion:" prefix, prioritized.

## Questions
Where context wasn't sufficient to evaluate.

## Praise
What's good in this change. Reinforce.
```

## Where to save

Save the review as `docs/architecture/reviews/YYYY-MM-DD-<short-summary>.md`. Also output to chat.

## Discipline

- **Don't lint.** Style and formatting are out of scope.
- **L2–L4 only.** Local correctness goes in a normal code review, not here.
- **Conformance to ADRs is the first thing.** A change that violates an accepted ADR without a superseding ADR is a blocking issue by default.
- **Favor a few high-leverage comments over a long list.** Architectural noise drowns out signal.
- **If the change is architecturally wrong at its core**, don't pile nitpicks. Write one detailed diagnosis in `Blocking issues` and suggest moving the discussion synchronous.

## When to recommend a new ADR

If the changes contain an implicit decision that isn't recorded — new dependency, new module, new pattern — flag it and suggest running `/krait_arch:adr` to capture it. Implicit decisions are how architectural debt accumulates.

## Output to chat

The structured review, plus the path of the saved file.
