<!--
  Keep this under one page. It is read in full at the start of every session,
  so every line competes with every other for attention.
  Delete anything that is generic good practice. Keep only what is true of
  THIS repository. Changes here are configuration changes: run the eval suite.
-->

# <Repo name>

<One sentence: what this service or application does.>

## Commands

<!-- TODO: Replace with the real, verified invocations. Run each one before
     writing it down. -->

| Task | Command |
|---|---|
| Install | `TODO:` |
| Build | `TODO:` |
| Test (all) | `TODO:` |
| Test (single file) | `TODO:` |
| Lint | `TODO:` |
| Format | `TODO:` |
| Typecheck | `TODO:` |
| Run locally | `TODO:` |

## Conventions

<!-- TODO: Only what differs from the language's default. Examples of the
     shape to aim for — replace all of these. -->

- Errors: `TODO: how errors are constructed, wrapped, and surfaced`
- Logging: `TODO: logger, levels, what must never be logged`
- Imports: `TODO: ordering, absolute vs relative, forbidden imports`
- Tests: `TODO: framework, file placement, naming, fixtures`
- Commits: `TODO: message format and branch naming`
- Types: `TODO: strictness rules, banned constructs`

## Architecture

<!-- TODO: The module map and the boundaries that matter. Name the rule that
     must not be broken, not just the folder list. -->

```
TODO: src/
        api/        ...
        domain/     ...
        infra/      ...
```

- `TODO: which layer may import which`
- `TODO: where business logic must live`
- `TODO: what talks to the database`

## Protected paths

Do not edit without explicit human instruction. Backed by hooks in
`.sdlc/guards/`:

- `TODO: db/migrations/**`
- `TODO: **/generated/**`
- `TODO: infra/**`
- `TODO: **/*.test.* during a bug fix`

## Common mistakes

<!-- TODO: Harvest these from repeated review comments in git history.
     State each as a rule, with the reason. -->

- `TODO: mistake -> rule -> why`
- `TODO:`
- `TODO:`

<!-- sdlc:begin — managed by agent-native-sdlc 1.1.0. Re-running the installer
     replaces everything between these markers; edit the rest of this file freely. -->
## Lifecycle

All work in this repository runs through five stages. Each stage reads the
artifact the previous stage committed and commits its own; together they are
the audit trail. A human decides at every gate.

| # | Stage | Read before starting | Produces | Gate — decided by |
|---|---|---|---|---|
| 1 | Spec | `.sdlc/playbook/1-spec.md` | `.sdlc/<slug>/spec.md` | Spec approved, flagged concerns resolved — Requester (+ tech lead for higher-risk items) |
| 2 | Build | `.sdlc/playbook/2-build.md` | `.sdlc/<slug>/plan.md`, then code and tests | Implementation plan interrogated and accepted — Engineer |
| 3 | Test | `.sdlc/playbook/3-test.md` | Passing tests, build, lint; green eval run when configuration changed | Tests, build, lint, and evals pass — Engineer |
| 4 | Deploy | `.sdlc/playbook/4-deploy.md` | `.sdlc/<slug>/review.md`, PR comments, a merged PR, a release | PR approved; production release authorized — Code owner; release manager |
| 5 | Maintain | `.sdlc/playbook/5-maintain.md` | A finding with evidence, promoted to a new `.sdlc/<slug>/spec.md` | Finding is real and worth acting on — On-call engineer |

**Which stage am I in?** Look in `.sdlc/<work-item-slug>/` and read the
`status:` in each artifact's front matter. No spec yet → stage 1. Spec
approved → stage 2. Plan approved and code written → stage 3. Green → stage 4.
Released → stage 5. `.sdlc/playbook/README.md` has the full routing.

**Before you act in any stage, read its playbook file and follow it.** Say out
loud which stage you are in, what you are producing, and who owns the gate.

### Rules that do not bend

- **No code without an approved spec and an accepted plan.** Both carry
  `status: approved` in their front matter. CI enforces this
  (`.sdlc/enforcement/spec_gate.py`); do not try to get around it.
- **Stop at every gate.** Present what you produced, name the decision, and
  wait. Never approve on a human's behalf, and never start the next stage in
  the same turn as the request for approval.
- **Never approve your own work.** You write; you review other people's
  changes; a human approves.
- **Production needs a named human authorization.** You cannot authorize your
  own deploy.
- **Never weaken a test to make it pass.** Bugs are fixed test-first: the
  failing test is committed on its own, then the fix, without touching it.
- **Findings loop back as specs**, never as silent patches.

### Recording a gate decision

```yaml
---
status: approved          # draft | in-review | approved | rejected
approved_by: <name>       # the human who owns this gate
approved_at: <YYYY-MM-DD>
---
```

Review passes for stage 4 are in `.sdlc/playbook/review-passes/`. Setting up
or changing the guardrails: `.sdlc/playbook/setup.md`.
<!-- sdlc:end -->
