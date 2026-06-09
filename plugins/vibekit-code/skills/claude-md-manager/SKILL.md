---
name: claude-md-manager
description: >-
  Create and maintain the CLAUDE.md project-context file that Claude Code reads
  first to understand a codebase — its stack, architecture, conventions, and
  current state. Use this WHENEVER a project has no CLAUDE.md (or a thin/stale
  one); when starting AI-assisted work on a repo; when the user says the AI
  "keeps forgetting", "makes the same mistake", "doesn't follow our patterns", or
  "ignores our conventions"; or when onboarding a new AI coding tool to an
  existing codebase. Trigger it proactively the first time you work in a repo
  that lacks a context document — a good CLAUDE.md is the cheapest quality
  upgrade available.
---

# CLAUDE.md Manager

## What this is for

An AI coding partner is brilliant but generic out of the box. It knows how to
write React; it does not know *your* folder layout, *your* naming conventions, or
that you decided last week to standardize on one error-handling pattern. Every gap
in that knowledge is a gap it fills with a plausible guess — which is where
"it keeps making the same mistake" comes from. The mistake isn't really the model's;
it's missing context.

CLAUDE.md is the fix: one file in the repo root that Claude Code reads automatically,
carrying the project's stack, architecture, conventions, and current state. This
skill does two jobs: **author** a good CLAUDE.md from an existing repo, and
**maintain** it so it stays accurate instead of rotting into a stale artifact nobody
trusts.

## Authoring a CLAUDE.md

1. Inspect the repo before writing. Read `package.json` / equivalent for the stack,
   scan the folder structure, open a couple of representative files to infer the
   real conventions (not the aspirational ones), and look for an existing schema /
   migration files for the data model. Derive the content from the code; don't ask
   the user things the repo already answers.
2. Fill the four core sections (template in `assets/claude-md-template.md`):
   - **Project overview** — name, one-line description, full stack
     (framework, DB, hosting, key libraries), and current status (what's built,
     in progress, next).
   - **Architecture** — folder structure and what lives where, the data model
     (tables and relationships), how API endpoints are structured, and the auth
     flow.
   - **Conventions** — naming (camelCase / PascalCase / etc.), code style, the
     standard component/module pattern, and the error-handling approach. State the
     conventions the code *actually* follows; if the code is inconsistent, say so
     and pick one to standardize on rather than documenting the mess.
   - **Current state** — what's being worked on now, known bugs / tech debt, and
     significant decisions with their rationale ("we chose X over Y because…").
3. Keep it accurate over aspirational. A CLAUDE.md that describes how you wish the
   code worked actively misleads the AI. Document reality; note the gap where reality
   and intent diverge.
4. Write it to the repo root as `CLAUDE.md` and tell the user what you put in the
   "decisions" and "known issues" parts so they can correct anything you inferred wrong.

## Maintaining a CLAUDE.md (the correction-codification loop)

The single most useful habit: **when you correct the AI on the same thing twice, the
rule isn't in CLAUDE.md yet — so put it there.** A correction made only in chat fixes
one output; a correction codified in the context file fixes every future output. Fix
the document, not just the conversation.

The loop, each time the AI produces something off-pattern:

1. **Spot** — notice the output doesn't match the project (e.g. it wrote a class
   component where the codebase uses function components with hooks).
2. **Correct** — say specifically what's wrong and what's wanted.
3. **Codify** — add the rule to the relevant CLAUDE.md section, in imperative form
   ("Always use function components with hooks; never class components").
4. **Verify** — have the AI redo the task and confirm it now matches.

Use the severity of a correction as a signal about *what* to fix:

- **Style** (naming, spacing): codify in Conventions; low stakes.
- **Pattern** (wrong file structure, wrong API shape): codify + verify; the doc was
  incomplete.
- **Logic / security**: stop and fix, and treat it as a sign the spec or context was
  too thin — fix the input, not just this output.
- **Architecture** (fundamentally wrong approach): full stop. The problem is upstream
  of CLAUDE.md — re-spec the work before continuing.

## When to update

Update CLAUDE.md whenever a significant architectural decision is made, a convention
changes, a new pattern is established, or a sprint's focus shifts. A stale context
file is worse than a short one — it confidently points the AI at things that are no
longer true. If a section has drifted from reality, fixing it is higher priority than
adding new detail.

## Output format

ALWAYS produce a `CLAUDE.md` matching the section structure in
`assets/claude-md-template.md`. Keep prose tight and skimmable — this is a reference
the AI parses on every session, not a narrative. Prefer short declarative rules over
explanation. Read the template before writing.
