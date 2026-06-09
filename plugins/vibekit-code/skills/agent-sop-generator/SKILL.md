---
name: agent-sop-generator
description: >-
  Generate a one-page operating spec (SOP) for an agent, automation, bot, or
  unattended service that's going into production. Use this WHENEVER the user is
  about to deploy or hand off an agent/automation/scheduled job/integration and
  needs to document how it behaves and who owns it; asks "how do I document this
  agent", "what's the runbook for this", or "make an SOP / spec for this
  automation"; or has several automations running and no record of what each one
  is allowed to do. Trigger it proactively when a build is being deployed without
  any ownership/permissions doc.
---

# Agent / Automation SOP Generator

## What this is for

The moment a second automation goes live, you have a management problem older than
software: who owns it, what's it allowed to touch, and what does the on-call person
read when it misbehaves at 6pm on a Friday. The fix is boring and effective — one
page per agent, written before it ships, that anyone can read cold.

The one-page limit is a real constraint, not a style note: **if the agent can't be
described on one page, it's too complex to put into production as-is.** That
complexity belongs in a staged rollout, not in mystery. Use that as a diagnostic
while filling the template — if a section sprawls, that's a signal the agent is
doing too much.

## How to generate the SOP

1. Gather the facts. Pull as much as possible from the code/config/conversation
   before asking the user — what it does, what tools/APIs it calls, what data it
   reads or writes, whether anything is scheduled or triggered.
2. Fill every field in the template (`assets/sop-template.md`). Do not leave fields
   blank — an unanswered field is itself a finding. If something is genuinely
   unknown, write `UNKNOWN — must resolve before production` so it stands out.
3. Apply the read/write discipline when filling permissions: the agent can read and
   draft freely, but flag every irreversible action (moving money, signing, external
   promises, personnel-data writes) as requiring a human approval gate.
4. Keep it to one page. If it won't fit, say so explicitly and recommend narrowing
   the agent's scope rather than shrinking the font.
5. Output the filled SOP as a markdown file the user can save next to the agent.

## The template

ALWAYS use the exact field set from `assets/sop-template.md`. The required fields:

- **Purpose** — one or two sentences. What it does and why it exists.
- **Allowed tools / systems** — exactly what it can call. Nothing implied.
- **Data classes touched** — what categories of data it reads or writes (public,
  internal, confidential, restricted/PII).
- **Read vs. write permissions** — per system: what it may read, what it may write,
  what it may never write.
- **Human approval gates** — which actions stop and wait for a human, and who that
  human is.
- **Logging expectations** — what gets recorded (inputs, outputs, decisions, errors)
  and where.
- **Failure & rollback** — what happens when it fails, and the exact steps to turn
  it off or roll it back.
- **Named owner** — a specific human, not a team. Someone whose name is on it.
- **Review cadence** — how often someone checks it's still earning its place, and
  what triggers an off-cadence review (e.g. any customer-facing incident).

Read `assets/sop-template.md` for the formatted template to fill and emit.

## Why each field earns its place

Don't treat these as bureaucracy. Purpose stops scope creep. Allowed tools + data
classes are what a security review needs. The read/write split and approval gates are
what keep an autonomous system from doing something irreversible. Logging is what
makes an incident reconstructable instead of mysterious. Owner + cadence are what stop
an agent from quietly running wrong for months because nobody felt responsible for it.
A field you're tempted to skip is usually the one that bites later.
