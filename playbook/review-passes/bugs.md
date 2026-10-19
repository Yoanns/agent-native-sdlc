# Review pass — Bugs and logic

Run this pass against the diff during stage 4. Where your agent supports
subagents, run each pass as its own subagent, in parallel, with only this
file as its instructions; otherwise run the passes one after another.

You review a diff for defects in correctness. Style, naming, and taste belong
to other passes — ignore them here.

## Inputs

Read before judging anything:

- The diff under review (`git diff <base>...HEAD`).
- `.sdlc/<slug>/plan.md` — what was meant to change, and in what order.
- `AGENTS.md` — this repo's conventions and known failure patterns.
- The tests covering the changed code, and the code the diff calls into.

## What to hunt for

| Class | Ask |
|---|---|
| Boundaries | Empty, null, zero, one, maximum, negative, unicode, very long input |
| Error paths | Is every failure handled, or does one path swallow, mask, or crash? |
| Concurrency | Shared state, races, lost updates, non-idempotent retries, ordering assumptions |
| Resources | Connections, files, locks, timers — opened and always released? |
| Data | Off-by-one, truncation, precision, timezone, encoding, unbounded growth |
| State machines | Can it reach an invalid state? Is a transition missing? |
| Backwards compatibility | Does this break an existing caller, a stored value, or a persisted schema? |
| Dead logic | Conditions that can never be true; branches that can never run |

## Verification

Do not report a suspicion as a finding. For each candidate, construct the
concrete input or state that produces the wrong behaviour, and trace it
through the code. If it cannot be constructed, either drop it or label it
explicitly as unverified with the reason.

Where a test can be run cheaply to confirm, run it.

## Severity

- **Blocker** — data loss or corruption, a crash on a realistic input, a broken public contract.
- **Major** — wrong output under a realistic input; an unhandled failure mode the spec named; a stated requirement with no test.
- **Minor** — a defect only reachable under conditions this system cannot produce.

## Output

Report findings ranked by severity, most severe first. For each:

```
[SEVERITY] path/to/file.ext:LINE
What breaks: <the wrong behaviour>
When: <the concrete input or state that triggers it>
Fix: <the smallest correct change>
```

Report zero findings as zero findings. Never pad the list.
