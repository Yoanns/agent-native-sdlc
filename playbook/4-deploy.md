# Stage 4 — Deploy

Review is multi-layer and runs before human attention, not instead of it.
Governance is enforced as the agent acts, not discovered afterwards.

## The one rule that cannot bend

**The agent writes; the agent reviews other people's changes; a human approves.**

The agent never approves its own work, never merges past branch protection, and
never authorizes a production release. Separation of duties is enforced by
branch protection and hooks, not by good intentions.

## Procedure

### 1. Check the branch carries its artifacts

The PR branch must contain the current `spec.md` and `plan.md`. Without them
the review passes have nothing to judge conformance against. If they are
missing or stale, fix that first.

### 2. Run the review passes in parallel

Read `REVIEW.md` for the passes this repo requires and their severity
thresholds. They are independent: run them in parallel as subagents where your agent supports that, otherwise one after another:

| Pass | Instructions | Asks |
|---|---|---|
| Bugs and logic | `.sdlc/playbook/review-passes/bugs.md` | Does it work? What input breaks it? |
| Security | `.sdlc/playbook/review-passes/security.md` | What can an attacker do that they should not? |
| Spec conformance | `.sdlc/playbook/review-passes/spec.md` | Does the diff do what `spec.md` and `plan.md` said, and nothing else? |

<!-- TODO: Add the passes specific to your organization — compliance,
     data classification, accessibility, licensing, performance budget —
     as further pass files in `.sdlc/playbook/review-passes/`, and list them in REVIEW.md. -->

### 3. Rank and record

Merge the passes' output into `.sdlc/<slug>/review.md`, ranked by severity, and
post it to the PR. For each finding: the file and line, what breaks and under
what input, and the smallest correct fix.

Apply the severity thresholds from `REVIEW.md`:

| Severity | Meaning | Effect |
|---|---|---|
| Blocker | Data loss, security hole, spec not met | Must be fixed before merge |
| Major | Wrong under a realistic input; missing test for a stated requirement | Fix, or a written decision to defer |
| Minor | Style, naming, non-load-bearing duplication | Author's discretion |

Report zero findings as zero findings. Padding a review with minor remarks to
look thorough trains people to skim it.

### 4. Address findings on the agent's own PRs

When the agent authored the PR, it fixes what the passes found and pushes. When a
human comments (or mentions the agent in a PR thread), respond in the thread with
the change made, or with the reason the finding does not hold — never silently.

### 5. Merge gate

A human code owner approves. Then merge triggers the pipeline. If findings
above the threshold are outstanding, say so plainly rather than letting the
approval carry them through.

Findings that are real but out of scope for this PR do not disappear: promote
each to a new work item with its own spec (see `.sdlc/playbook/README.md`).

### 6. Release gate

Production deploys require a named human authorization, enforced by a hook —
not by a checkbox and not by the agent's judgement. Staging is controlled; local
is free. See the autonomy tiers in `.sdlc/playbook/README.md`.

Before the first automated deploy, rehearse the rollback path and prove it
works. An automated deploy without a proven rollback is an automated incident.

### 7. Feed the gaps back

A finding that got through to production, or a policy gap the passes missed,
goes back into the guardrails: `AGENTS.md` for conventions, a rule for
consistently applied policy, a hook for policy that must always hold,
`REVIEW.md` for a missing pass. Configuration changes run the evals.

## The agent in the pipeline

The agent runs non-interactively in CI for the judgement steps that scripts do
badly: triaging a failure to the likely commit, drafting a changelog from
merged PRs, summarizing what a release contains.

Constraints for pipeline runs:

- Sandboxed execution with a network allowlist.
- Scoped, short-lived credentials — never the deploy key.
- Deployment actions exposed as MCP tools with typed inputs, not as shell
  scripts the agent composes freely.

<!-- TODO: Name your CI system, the deploy MCP server or tooling, the branch
     protection rules in force, and who may authorize a production release. -->
