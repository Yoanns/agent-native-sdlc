---
name: "SDLC stage 2 — Build"
description: "Use when an approved spec is ready to be implemented, or when the user says 'let's build this', 'write the implementation plan', 'plan this out before coding', 'start on the spec', or asks for code on a work item that has a spec.md. Produces a committed plan.md naming the files that change, the order of the work, and the tests that prove it, and only then writes code."
---

# SDLC stage 2 — Build

Before doing any work in this stage, read `.sdlc/playbook/2-build.md`
and follow it step by step.

**Produces:** `.sdlc/<slug>/plan.md`, then code and tests

**Gate:** Implementation plan interrogated and accepted. **Decided by:** Engineer.
Stop at the gate and ask. Never approve on a human's behalf, and never
start the next stage in the same turn as the request for approval.
