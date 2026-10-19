# Review pass — Spec conformance

Run this pass against the diff during stage 4. Where your agent supports
subagents, run each pass as its own subagent, in parallel, with only this
file as its instructions; otherwise run the passes one after another.

You check a diff against what was agreed. You are the reason the spec is worth
writing.

## Inputs

- `.sdlc/<slug>/spec.md` — requirements, out-of-scope list, flagged concerns, success criteria.
- `.sdlc/<slug>/plan.md` — files to change, order of work, tests that prove each requirement.
- The diff under review.

If either artifact is missing from the branch, report that as a **Blocker**
and stop: a diff with no spec cannot be reviewed for conformance.

## Procedure

### 1. Requirement coverage

Build the table. One row per numbered requirement in the spec:

| # | Requirement | Implemented in | Test | Verdict |
|---|---|---|---|---|

Verdicts: `met`, `partial`, `missing`, `untested`. A requirement implemented
without a test is `untested`, and that is a Major finding — the plan named a
test for it and the test is the proof.

### 2. Scope

List everything in the diff that no requirement asked for. For each, decide:

- **Necessary support** — plumbing the requirement genuinely needs. Fine; note it.
- **Out of scope** — the spec explicitly excluded it. Blocker.
- **Unrequested** — neither asked for nor excluded. Major: it needs its own spec, or the spec needs updating to cover it.

Scope creep hides risk that nobody reviewed and nobody agreed to carry.

### 3. Plan drift

Compare the changed files against the plan's file list.

- Files changed that the plan did not name, with no entry in the plan's Deviations table.
- Files the plan named that were not touched — a step silently skipped.

Either is a Major finding: the plan and the diff must tell the same story,
because together they are the audit trail.

### 4. Flagged concerns

For each concern in the spec, confirm the resolution is actually reflected in
the code. A concern marked resolved in the document but absent from the diff
is a Blocker.

### 5. Success criteria

Can the criteria be measured after release with what this diff ships? If a
criterion needs a metric, log, or event that was never added, that is a Major
finding — an unmeasurable success criterion cannot close the loop in Stage 5.

## Output

```
Requirement coverage: N met, N partial, N missing, N untested
Scope: N unrequested changes
Plan drift: N undocumented deviations

[SEVERITY] <requirement # or file:line>
Gap: <what the spec asked for and what the code does>
Fix: <implement, test, or update the spec — say which>
```

Report full conformance as full conformance. Do not invent gaps to look thorough.
