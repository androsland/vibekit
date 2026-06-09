---
name: spec-first-build
description: >-
  Turn a vague build request into a constrained, buildable spec BEFORE writing
  code. Use this WHENEVER the user describes something they want built in loose
  terms ("build me a tool that...", "make an app for...", "automate this") without
  naming concrete inputs, data sources, constraints, or success criteria; or when
  a first attempt produced something plausible-but-wrong and the user is frustrated
  it "made stuff up". Trigger it proactively at the start of any non-trivial build
  when the requirements are underspecified — pinning the spec down first is what
  prevents confident, wrong output.
---

# Spec-First Build

## What this is for

Most "the AI made things up" failures aren't model failures — they're
under-specification. A vague request leaves an enormous answer space, and the system
fills the gaps with whatever it has. The narrower and more concrete the inputs, the
less there is to invent.

The classic illustration: "Where can I see a butterfly?" has a billion answers, none
useful. "Where can I see a monarch butterfly in [town] on [date]?" has one — because
species, place, and date collapsed the answer space to something actionable. Same
model, same capability; the difference is the constraints in the question.

This skill front-loads that constraint-setting. Before any code is written, produce a
spec that names the sources, fixes the parameters, and states what success and
failure look like — so the build aims at one concrete target instead of an averaged
guess.

## When to use it vs. just building

Use it when the request is open-ended, touches real data or external systems, or has
non-obvious success criteria. Skip it for genuinely trivial, self-contained tasks
where the spec would be longer than the code. When in doubt on anything that will
touch users or persist data, spec first — the cost is a few minutes, and the failure
it prevents is building the wrong thing confidently.

## How to run it

1. Take the user's request as the seed. Identify what's *not* pinned down: inputs,
   data sources, the shape of the output, who uses it, what "correct" means, what
   should explicitly NOT happen.
2. Fill the spec template (`assets/spec-template.md`). Where the user hasn't said,
   propose a concrete default and mark it `[ASSUMPTION]` so they can correct it —
   don't leave it open, and don't silently guess.
3. Ask the user to confirm or correct the assumptions before building. Keep it to one
   focused round of questions, not an interrogation — surface the assumptions you
   made and let them react.
4. Once confirmed, the spec becomes the build target. Build to it. If reality forces
   a deviation, update the spec rather than drifting silently.

## The spec template

ALWAYS use the structure in `assets/spec-template.md`. The sections:

- **Goal** — one sentence. What this is and who it's for.
- **Inputs & data sources** — named, specific. Where does data actually come from?
  What format? What's authoritative? Vague inputs are the #1 source of invented output.
- **Constraints & parameters** — the concrete bounds: ranges, formats, limits, the
  exact rules the thing must follow.
- **User stories, data model, UI flow, API contract** — fill these for features and
  apps (skip the ones that don't apply to a small script). User stories pin down who
  does what and why; the data model and API contract are where vague specs invent the
  wrong structure; the UI flow stops the build from guessing the user journey.
- **Output** — what it produces, in what shape, for whom.
- **Success criteria** — how you'd know it's working correctly. Testable if possible.
- **Failure modes** — what happens if it fails, who's affected, what the fallback is.
  (This is the "what happens if it fails?" question, asked before the code exists.)
- **Out of scope** — what this explicitly does NOT do. Naming non-goals prevents
  scope creep and stops the build from inventing features.
- **Open assumptions** — everything marked `[ASSUMPTION]`, collected for the user to
  confirm.

Read `assets/spec-template.md` for the formatted version to fill and emit.

## The principle to carry through

Specificity beats volume. Narrow the inputs and you narrow the failure modes. A spec
that names its sources, fixes its parameters, and states its non-goals gives the build
enough truth to stop guessing — which is the entire point.
