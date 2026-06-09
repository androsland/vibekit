---
name: calibration-battery
description: >-
  Run a short battery of checks that confirm Claude actually understands a
  codebase BEFORE building features in it — conventions, data model, existing
  patterns, error handling, and deploy flow. Use this WHENEVER starting
  AI-assisted work on an unfamiliar or inherited repo; right after writing or
  updating a CLAUDE.md; when onboarding a new AI tool to a project; or when the
  user worries the AI "doesn't really get" their codebase yet. If the repo has no
  CLAUDE.md yet, use claude-md-manager FIRST and run this battery afterward to
  verify it — don't compete with it. Users rarely ask for this by name, so trigger
  it proactively before a first real feature on a project Claude hasn't worked in;
  thirty minutes here saves hours of off-pattern code later.
---

# Calibration Battery

## What this is for

Out of the box an AI coding partner is capable but generic — it knows the language,
not your project. Before trusting it with real features, it's worth a quick,
controlled check that it has absorbed the project's actual conventions and shape.
Each miss is a cheap, early signal that the context document (CLAUDE.md) is thin in
a specific place — and far better caught now than discovered three features deep when
the off-pattern code is already load-bearing.

This is a fast diagnostic, not a build session. Run it, score it, and turn every miss
into a CLAUDE.md fix.

## How to run it

Work through the five tests below in order. For each, do the small task, then compare
the output against what the codebase actually does. Record pass / miss and, for every
miss, the specific rule that should be added to CLAUDE.md so the gap closes
permanently. Don't fix the conventions silently — the point is to surface what the AI
didn't know, then codify it.

If a `claude-md-manager` skill is available, hand misses to it to update the context
document. The two are designed to work as a loop: calibrate → find gaps → codify →
re-calibrate.

## The five tests

**1. Convention check.** Ask for a trivial new component or module (e.g. a button, a
small util). Does it match the project's naming, file placement, and code style?
A miss means the Conventions section of CLAUDE.md is incomplete.

**2. Data-model echo.** Ask it to describe the data model back — tables/entities,
relationships, key constraints. Wrong or vague means the Architecture section needs
the real schema, not a guess.

**3. Pattern replication.** Point it at one existing component/module and ask for a
similar sibling. Does the new code mirror the original's structure, error handling,
and styling? A miss means the "standard pattern" isn't written down anywhere it can see.

**4. Error-handling check.** Ask it to add error handling to a function. Does it follow
the project's actual approach (toast vs. error boundary vs. logging vs. typed result)?
A miss means the error-handling convention is unstated.

**5. Deploy awareness.** Ask what happens when code is pushed to the main branch. Does
it know the CI, preview deploys, and environment-variable setup? A miss means the
deploy/infra context is missing — which is exactly the knowledge that prevents
"works locally, breaks in prod."

## Output format

ALWAYS produce a short scorecard:

```
# Calibration Battery: [project]

| Test | Result | Gap → CLAUDE.md fix |
|------|--------|---------------------|
| Convention check | pass / miss | [rule to add, if miss] |
| Data-model echo | pass / miss | [...] |
| Pattern replication | pass / miss | [...] |
| Error handling | pass / miss | [...] |
| Deploy awareness | pass / miss | [...] |

## Verdict
[Ready to build / fix CLAUDE.md first — and which sections.]
```

## When to re-run

Re-run after any major milestone, after a significant architecture change, or when
onboarding a different AI tool to the same project. A clean battery is the green light
to start building features with confidence; a battery full of misses is a sign to fix
the context document before writing anything load-bearing.
