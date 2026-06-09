---
name: production-readiness-review
description: >-
  Audit a working-but-unfinished build (especially AI-assisted / "vibe-coded"
  prototypes) for the production gaps that prototypes never need but real users
  expose. Use this WHENEVER the user is about to ship, deploy, launch, or "go
  live"; asks "is this ready for users / production?"; hands over a repo that
  "works on my machine"; or mentions auth, payments, multi-tenancy, data
  isolation, failover, logging, or compliance for something they built quickly.
  Trigger it even if the user only says "can you review this before I ship" —
  this is the review they mean. This is a pre-ship/launch readiness sweep, NOT
  active-incident debugging: if something is currently broken or failing in prod,
  use systematic-debugging instead.
---

# Production-Readiness Review

## What this is for

A prototype that demonstrates the idea and a system that survives real users are
two different artifacts. The prototype solves the problem; it rarely survives the
deployment, because the parts that make it survivable — identity, isolation,
failure handling, logging — were never needed while one trusted person ran it on
one laptop. AI-assisted builds reach "it works" faster than ever, which means the
gap between *works* and *safe to ship* is now the most common place things break.

This skill runs a structured audit across the dimensions where that gap lives, and
produces a findings report ranked by blast radius so the user knows what to fix
before a single real user touches it.

## The governing question

Run the whole review against one question: **"What happens if it fails?"**

For every component, the user should be able to answer concretely:
- Who gets hurt, and how?
- What's the blast radius — one user, all users, money, reputation, legal?
- What's the failover / fallback when this breaks at 2am on a Saturday?
- If a human task was automated away, how does anyone remediate when it fails?

If those can't be answered with specifics, that component is not ready, regardless
of how well it demos. Use the answers to rank findings: highest blast radius +
weakest fallback gets fixed first.

## How to run the review

1. Read the codebase (or the description, if no code is available). Identify the
   actual surfaces: where users authenticate, where data is stored, where money
   or irreversible actions happen, what's exposed to the network.
2. Walk every dimension in the checklist below. For each, record the current state
   and the gap, not just pass/fail.
3. Apply the **read vs. write line** (see below) to every action the system can take.
4. Produce the findings report in the exact structure given at the end.
5. Do NOT silently fix things mid-review unless asked — the value here is the
   honest inventory of what's missing. Offer to fix after the user sees the list.

## The audit dimensions

Work through each. The "common prototype gap" notes are where these usually fail —
look there first.

**1. Identity & authentication.** Is there real auth, or an implicit "only I use
this" assumption? Are sessions/tokens handled safely? Common prototype gap: no
auth at all, or auth that protects the UI but not the API behind it.

**2. Authorization & data isolation (multi-tenancy).** When there are N users, does
each one see only their own data? Is isolation enforced server-side, or only by the
client not asking for other rows? Common prototype gap: a single shared data space
that worked fine with one user and silently leaks across users at scale.

**3. Irreversible actions & the read/write line.** See the dedicated section below.

**4. Failure & failover.** What happens when a dependency (DB, API, model provider,
payment processor) is down or slow? Are there timeouts, retries, graceful
degradation, a fallback path? Common prototype gap: the happy path is the only path;
any failure surfaces as a stack trace or a hang.

**5. Logging & observability.** Are actions logged enough to reconstruct "what
happened" after an incident — inputs, outputs, decisions, errors, who/what triggered
them? Can you tell, after the fact, that an automation was running fine but producing
garbage? Common prototype gap: console logs only, nothing persisted, no way to audit.

**6. Secrets & configuration.** Are API keys, credentials, and connection strings
kept out of the code and the client bundle? Are insecure defaults (open buckets,
permissive CORS, default admin creds, debug mode on) turned off? Common prototype
gap: keys hardcoded or shipped to the browser; storage left world-readable.

**7. Input handling & abuse.** Is user input validated and sanitized before it hits
a DB, a shell, a model prompt, or another system? Is there rate limiting on anything
expensive or destructive? Common prototype gap: trusting input because "I'm the only
one typing."

**8. Data handling & compliance.** What sensitive data does this touch (PII, IDs,
health, financial)? Is it stored only where it must be, encrypted where appropriate,
and deletable? Are there obligations (consent, retention, regional rules) the build
ignores? Common prototype gap: collecting and storing far more than needed, with no
deletion path — the kind of gap that turns a safety app into a breach.

**9. Rollback & kill switch.** Can this be turned off fast if it goes wrong? Is there
a documented rollback, and is it tested? What breaks when you flip the switch?
Common prototype gap: no off switch — disabling it means redeploying or editing the
database by hand under pressure.

## The read/write line

The single most useful guardrail for AI-driven and automated systems:

**Let the system read enough to be useful. Do not let it write enough to be dangerous.**

Reading, drafting, calculating, organizing, queuing — fine to automate. But any
action that is hard to undo should stop at a human:

- Anything that moves money → a human confirms the send.
- Anything that signs or commits to a contract → a human signs.
- Anything that makes an external promise to a customer → a human approves.
- Anything that touches employee/personnel data → restricted read, no autonomous write.

When auditing, trace every action the system can take to completion and flag any
path where it executes an irreversible action with no human checkpoint. The system
can populate the form, draft the message, prepare the transaction — a human presses
send.

## Output: the findings report

ALWAYS produce the report in this structure:

```
# Production-Readiness Review: [project name]

## Verdict
[One line: ready to ship / not ready / ready for limited rollout only — and why.]

## Critical (fix before any real user)
For each finding:
- **[Dimension] — [short title]**
- What's missing / wrong:
- What happens if it fails: [who's hurt, blast radius, current fallback]
- Suggested fix:

## Important (fix before broad rollout)
[Same structure]

## Minor / later
[Same structure, terse]

## What's already solid
[Brief — name the things done right, so the user knows what NOT to touch.]
```

Rank findings by blast radius and weakness of fallback, not by how easy they are to
fix. The scary findings are the high-impact ones with no failover, even if they're
quick to patch.

## Concrete itemized checks

The dimensions above are the reasoning; these are the specific line items to verify
on a typical web/app build. Treat the security group as non-negotiable — shipping
without these isn't lean, it's a liability. Not every item applies to every stack;
note the ones that are N/A rather than skipping silently.

**Security (non-negotiable):**
- Input validated on every user-facing form and API endpoint.
- Rate limiting on authentication endpoints (and anything expensive or destructive).
- No secrets or env vars exposed in client-side code or the shipped bundle.
- HTTPS enforced everywhere.
- Any keys ever committed by accident have been rotated.
- Auth tokens expire and rotate correctly; sessions can be invalidated.
- Error messages don't leak stack traces, internal paths, or implementation detail.

**Infrastructure (non-negotiable):**
- Production environment variables set in the host (not only locally).
- Custom domain configured and resolving (if applicable).
- Error monitoring / logging active and actually capturing events.
- Database backups configured and restorable (a backup you can't restore isn't one).
- Build passes clean — no warnings treated as acceptable noise.

**User experience (non-negotiable):**
- Core user flow works end-to-end with real data.
- Error states handled gracefully — no blank screens or silent failures.
- Loading states shown during async operations.
- Mobile responsive (for web apps).
- Privacy policy and terms of service published (if collecting any user data).

A block on any security item is the signal to stop and get a human engineer or
security reviewer involved before launch — not to ship and fix later.

## Scope note

This is a hygiene checklist, not a substitute for a security professional. For
high-stakes builds (real payments, regulated data, large user counts), say so plainly
and recommend a human security review / penetration test before launch — these checks
surface the obvious, repeatable gaps, not every vulnerability.
