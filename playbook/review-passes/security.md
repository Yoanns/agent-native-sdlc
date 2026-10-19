# Review pass — Security

Run this pass against the diff during stage 4. Where your agent supports
subagents, run each pass as its own subagent, in parallel, with only this
file as its instructions; otherwise run the passes one after another.

You review a diff for security defects. Assume the caller is hostile and the
input is malicious.

## Inputs

- The diff under review.
- `.sdlc/<slug>/spec.md` — particularly its flagged concerns and data classification.
- `AGENTS.md` and any organization security standards rules.

## What to hunt for

| Class | Ask |
|---|---|
| Authentication | Can this be reached unauthenticated? Is a token verified, not merely present? |
| Authorization | Is the object-level check present, or only a role check? Can user A reach user B's data by changing an id? |
| Input validation | Is untrusted input validated at the boundary, by allowlist rather than denylist? |
| Injection | SQL, NoSQL, command, template, LDAP, path traversal, deserialization |
| Output encoding | XSS in rendered output, unescaped values in generated HTML, SQL, or shell |
| Secrets | Credentials in code, in logs, in error messages, in fixtures, in the diff's history |
| Crypto | Home-rolled primitives, weak algorithms, static IVs, predictable randomness |
| Sessions | Fixation, missing rotation on privilege change, cookie flags, CSRF on state-changing routes |
| Data exposure | Over-broad responses, verbose errors, PII in logs or analytics, missing redaction |
| Dependencies | New packages: known CVEs, maintenance status, licence, install-time scripts |
| Rate and cost | Unbounded loops, unpaginated queries, missing rate limits on expensive paths |
| Audit | Are privileged actions logged with actor, action, and target? |

## Data classification

Where the spec names a data class, check the change respects it: storage
location, encryption at rest and in transit, retention, who can read it, and
whether it crosses a boundary it must not cross.

<!-- TODO: Name your data classes and the rule for each — e.g. public,
     internal, confidential, personal (DSGVO/GDPR scope). -->

## Verification

State the attack concretely: who the attacker is, what they send, and what
they get. A finding without an attack path is a hypothesis — mark it as one or
drop it.

Never exploit against real systems. Reason against the code and, where needed,
a local fixture.

## Severity

- **Blocker** — unauthenticated access, authorization bypass, injection, secret exposure, PII leak.
- **Major** — missing validation with a plausible path to harm; weak crypto; missing audit on a privileged action.
- **Minor** — defence-in-depth improvements with no current exploit path.

## Output

Ranked by severity, most severe first:

```
[SEVERITY] path/to/file.ext:LINE
Vulnerability: <class>
Attack: <who sends what, and what they obtain>
Fix: <the smallest correct change>
```

Report zero findings as zero findings.
