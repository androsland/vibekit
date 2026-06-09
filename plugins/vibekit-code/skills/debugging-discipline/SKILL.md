---
name: debugging-discipline
description: >-
  A lightweight, self-contained debugging discipline for everyday "it's broken" —
  likeliest cause first, one change at a time, a hard stuck-loop trigger when going
  in circles. Needs no planning workflow, session-state files, or extra tooling; it's
  the default for a normal single-session bug. Use this WHENEVER the user reports a
  bug, an error, "it's broken", "works locally but fails in prod", an infinite
  spinner, a failing build, or a test that won't pass; and ESPECIALLY when they say
  they've been stuck on the same issue for a while or keep trying the same fix.
  Trigger it proactively the moment a debugging loop starts repeating itself. This
  owns ACTIVE failures
  (something is broken right now); a pre-ship readiness sweep with nothing currently
  failing belongs to production-readiness-review. Prefer this over heavier stateful
  or workflow-based debug processes for an ordinary bug; reach for
  those only when a formal multi-session debugging workflow is already in use.
---

# Debugging Discipline

## What this is for

Unstructured debugging fails in two ways: changing several things at once (so you
can't tell what fixed or broke it), and grinding on the same dead-end theory long
past the point of progress. This skill imposes a simple discipline — work from the
most likely cause outward, change one variable at a time, and escalate deliberately
when stuck — so a bug gets cornered instead of chased.

## The method

1. **State the gap precisely.** Write down expected behavior, actual behavior, exact
   steps to reproduce, and the verbatim error (if any). A bug you can't state clearly
   is one you don't understand yet — and vague inputs produce vague fixes. Half of all
   bugs become obvious the moment they're stated this precisely.
2. **List likely causes, most-probable first.** Don't jump to the exotic explanation.
   Order the candidates by probability given the symptom, and work down the list.
3. **Test one cause at a time.** Form a single hypothesis, make the smallest change
   that would confirm or kill it, and check. Never change multiple things at once —
   if it starts working, you won't know why, and "works for unknown reasons" is just
   a future bug.
4. **Read the actual error.** Error messages and line numbers usually point near the
   cause. For an infinite spinner, check the console/network for the unhandled
   promise or failed request. For "works locally, fails in prod," suspect environment
   variables and config differences first.
5. **Keep a running log.** Note what was tried and ruled out, so the search narrows
   instead of circling back to already-eliminated theories.

## The stuck-loop trigger

If the same issue has had no progress after about 30 minutes — or the same fix has
been attempted more than once — stop. That's a stuck loop, and continuing the same
approach won't break it. Do one of these instead:

- **Change altitude.** Step back and challenge the approach itself: is the whole
  strategy wrong? Is there a simpler path that sidesteps the problem entirely?
- **Re-state from scratch.** Re-describe the problem fresh, as if to someone new —
  what's tried, what's been ruled out, the current theory. Re-stating frequently
  exposes a wrong assumption baked in from the start.
- **Reduce to a minimal repro.** Strip the problem to the smallest case that still
  fails. The reduction itself usually reveals the cause.
- **Escalate.** If it touches security, payments, data integrity, or architecture, a
  human engineer should weigh in rather than letting an AI thrash on high-stakes code.

## Common causes worth checking early

These recur often enough to check near the top of the list:

- Build fails after edits → syntax error or a missing/incorrect import; read the
  reported line.
- Works locally, fails in prod → environment variables not set in the host.
- AI keeps repeating the same wrong pattern → the correct convention isn't in the
  project context (CLAUDE.md); fix the doc, not just this output.
- Outdated API/framework usage → state the current version and provide current docs.
- Stale preview / old version showing → build cache or wrong branch; force a clean
  redeploy.
- Infinite loading → unhandled async/promise; check the console and add error handling.

## Output format

Walk the user through it conversationally, but make the structure visible: state the
gap, show the ranked causes, test the top one, report the result, and either continue
down the list or declare a stuck loop and switch tactics. End with the root cause and
the specific fix once found — and if a missing convention caused it, note the CLAUDE.md
rule that would prevent a recurrence.
