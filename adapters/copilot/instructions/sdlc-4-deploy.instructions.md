---
name: "SDLC stage 4 — Deploy"
description: "Use when a change is ready for review or release, or when the user says 'review this PR', 'run the review passes', 'ready to merge', 'ship it', 'deploy this', or mentions release authorization. Runs the agentic review passes defined in REVIEW.md in parallel, ranks findings by severity, and enforces that a human — never the agent — approves and authorizes production."
---

# SDLC stage 4 — Deploy

Before doing any work in this stage, read `.sdlc/playbook/4-deploy.md`
and follow it step by step.

**Produces:** `.sdlc/<slug>/review.md`, PR comments, a merged PR, a release

**Gate:** PR approved; production release authorized. **Decided by:** Code owner; release manager.
Stop at the gate and ask. Never approve on a human's behalf, and never
start the next stage in the same turn as the request for approval.
