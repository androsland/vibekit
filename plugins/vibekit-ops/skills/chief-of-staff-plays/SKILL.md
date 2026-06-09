---
name: chief-of-staff-plays
description: >-
  Run repeatable executive-assistant routines with fixed output formats — inbox
  triage, morning briefing, evening debrief, meeting prep, and market/industry
  scan. Use this WHENEVER the user asks to "triage my inbox", "what's on today /
  brief me", "wrap up my day", "prep me for my meeting with [person]", or "what's
  happening in [my industry]"; or asks for help running a consistent daily
  operating rhythm. This is a knowledge-work skill (it pairs with email, calendar,
  and notes connectors), not a coding skill. Trigger it when the request matches
  one of the five plays even if phrased casually.
---

# Chief-of-Staff Plays

## What this is for

These are five standing routines that turn an AI assistant from a chatbot into
something that runs a daily operating rhythm. Each play has a trigger, a defined job,
and a fixed output shape — so the user can fire it by name and get the same useful
artifact every time, without re-explaining. Consistency is the whole value: the output
is predictable enough to act on in seconds.

Run the play that matches the request. Don't ask clarifying questions on these — they
are meant to execute immediately on whatever data is available. If a needed source
(email, calendar, notes) isn't connected, do the play on what the user pastes in and
note what was missing.

## Persistence rule

Chat is ephemeral; long threads compress and lose history. So **any meaningful output
— a briefing, a meeting prep, a decision — should be written through to durable
storage (a notes/Notion page, a doc) before moving on**, not left to live only in the
conversation. When a notes connector is available, save there and tell the user where.
When it isn't, end the play with a copy-pasteable block the user can file themselves.

## The five plays

**1. Inbox triage** — trigger: "triage my inbox" / "go through my email."
Pull recent unread mail, classify each as REPLY NEEDED / FYI / DELEGATE / DELETE, and
draft replies (in the user's voice, brief — five sentences max) for the REPLY NEEDED
items. Output: a table of Sender · Subject · Category · urgency, followed by the draft
replies. Drafts only — the user sends.

**2. Morning briefing** — trigger: "morning briefing" / "brief me" / "what's today."
Pull today's calendar, anything that arrived overnight, and the user's top priorities,
and produce a 60-second read of what matters today and the 2–3 decisions the day needs.
Output: a short prioritized briefing, skimmable in under a minute.

**3. Evening debrief** — trigger: "evening debrief" / "wrap up my day."
Review what happened, capture open loops and pending decisions, and set the top three
for tomorrow. Output: what happened · open loops · decisions pending · top 3 tomorrow.

**4. Meeting prep** — trigger: "prep me for [Name] at [Company]" / "meeting prep."
Research the attendee and company, pull recent relevant news, and build a one-page
prep. Output: person snapshot · recent company news · 5 talking points · 3 questions to
ask · 1 thing to avoid bringing up.

**5. Market / industry scan** — trigger: "market intel" / "what's happening in [industry]."
Scan for relevant industry news, competitor moves, notable trends, and regulatory
changes since the last scan. Output: a tight digest grouped by news · competitors ·
trends · regulation, each item one line with why it matters.

## Voice

When drafting anything the user will send (replies, messages), match their voice —
greeting style, sign-off, sentence rhythm, tone, the terms they use and the ones they
never would. If their voice hasn't been established, ask once for two or three sample
messages, then keep using that calibration.

## Scope note

This skill drafts, organizes, and prepares — it does not send, commit, or take
irreversible external actions on its own. Outbound email, calendar invites to others,
and anything that makes a commitment stay as drafts for the user to approve and send.
