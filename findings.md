# Blind Dispatch Evaluation — vibekit-code & vibekit-ops

_Date: 2026-06-09 · Method: blind subagent dispatch + behavior conformance test_

> ## ✅ What was fixed (post-evaluation)
>
> The evaluation below found **one** problem: `vibekit-code:systematic-debugging` lost **0/3** of its trigger cases because its name collided with `superpowers:systematic-debugging` and it was out-competed by `gsd-debug`. That has been fixed:
>
> - **Renamed** `systematic-debugging` → **`debugging-discipline`** (now invoked as `vibekit-code:debugging-discipline`) — kills the identical-name collision. Directory, `name:` frontmatter, and body title all updated.
> - **Differentiated the description** with a "lightweight, self-contained, no planning workflow / session-state files / extra tooling — the default for a normal single-session bug" wedge, plus a closing "prefer this over heavier stateful or workflow-based debug processes" clause — to beat `gsd-debug`.
> - **Cross-reference updated** in `production-readiness-review` ("use debugging-discipline instead").
> - **README** skill list + example invocation updated.
> - **Version bumped** `1.0.0 → 1.1.0` (`vibekit-code/.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json`) so users receive the update.
> - **Structurally validated** all 7 skills against skill-creator's `quick_validate.py` rules (frontmatter keys, kebab-case name ≤64, description ≤1024, no angle brackets). This caught the rewritten `debugging-discipline` description at **1041 chars (over the 1024 limit)** — trimmed to 973 chars without losing the wedge. **All 7 now pass.**
>
> **Focused rerun result: 0/3 → 2/3.** The rename recovered both cases `gsd-debug` was stealing (the stuck-loop and works-locally-fails-in-prod cases — the skill's signature wedge). The one remaining loss (a plain "build broke" prompt) now goes to `superpowers:systematic-debugging`, a genuine near-duplicate skill — structural redundancy between two installed debuggers, not a defect fixable by description. The decoy boundary held (still routes to `production-readiness-review`). Behavior was unchanged by the edit (body is identical), so the 7/7 behavior pass still stands.
>
> The original evaluation that drove these changes is preserved below, unedited in substance but annotated to reflect the new name.

---

## Summary

| Result | Count (as-evaluated) | After fix |
|---|---|---|
| Skills evaluated | 7 (6 in `vibekit-code`, 1 in `vibekit-ops`) | — |
| Trigger cases passed | 18 / 21 | **20 / 21** |
| False positives on decoys | 0 / 14 | 0 / 14 |
| Behavior outputs matching SKILL.md | 7 / 7 | 7 / 7 (unchanged) |
| Skills needing a fix | **1** (`systematic-debugging` — name collision) | **0 outstanding** (fixed; 1 residual near-duplicate overlap, see note) |

**Headline (as-evaluated):** 6 of 7 skills are correctly calibrated — they trigger on their cases, cede every near-miss to the right neighbor, never false-fire on unrelated prompts, and produce exactly the artifact their SKILL.md promises. The lone failure was `vibekit-code:systematic-debugging`, which lost **0/3** of its trigger cases — not because of a weak description, but because its **name was identical to `superpowers:systematic-debugging`** and it was out-competed by `gsd-debug` on its own signature cases.

**Post-fix:** renamed to `vibekit-code:debugging-discipline` + differentiated description → **2/3** on rerun (recovered both `gsd-debug` losses). The single residual loss is to the near-identical `superpowers:systematic-debugging` and is structural overlap, not a description defect. See the "✅ What was fixed" banner above.

---

## 1. Inventory & realistic competition

The full installed skill set (~150 skills) was given to every blind router as the candidate pool. Most (gsd-* command family, vercel:*, supabase:*, design/image-gen, etc.) were noise. The rivals that actually competed for your cases:

| Your skill | Closest installed rivals |
|---|---|
| `vibekit-code:spec-first-build` | `superpowers:brainstorming`, `gsd-spec-phase`, `gsd-new-project`, `feature-dev:feature-dev` |
| `vibekit-code:calibration-battery` | `vibekit-code:claude-md-manager`, `gsd-map-codebase`, `arch-review` |
| `vibekit-code:systematic-debugging` → renamed `debugging-discipline` | ⚠️ **`superpowers:systematic-debugging` (identical name)**, **`gsd-debug`** |
| `vibekit-code:production-readiness-review` | `security-review`, `arch-review`, `code-review`, `gsd-secure-phase` |
| `vibekit-code:agent-sop-generator` | `vibekit-code:claude-md-manager`, `gsd-docs-update` |
| `vibekit-code:claude-md-manager` | **`claude-md-management:revise-claude-md`**, `claude-md-management:claude-md-improver`, `remember:remember` |
| `vibekit-ops:chief-of-staff-plays` | `deep-research`, `resend:agent-email-inbox`, `schedule` |

### Method note (data quality)
Early general-purpose subagents sometimes refused to `Read` the inventory file and answered from memory (3 runs, one hallucinating that the file "does not exist"). Fix: the inventory was **embedded inline** in every subsequent prompt so a real read was guaranteed. The two affected trigger cases (SFB-1, SFB-2) were re-run inline and both then resolved correctly.

---

## 2 & 3. Test cases + blind trigger results

Legend: ✅ routed to your skill · ❌ stolen by a rival · ✓decoy correctly routed to the intended neighbor · ✓ correctly returned "none".

### spec-first-build — **3/3 PASS**
| Type | Prompt (abridged) | Expected | Blind pick | |
|---|---|---|---|---|
| trigger | "Build me a tool that sorts incoming customer emails into categories" | spec-first-build | spec-first-build | ✅ |
| trigger (casual) | "make an app for my gym to handle bookings — just start building it" | spec-first-build | spec-first-build | ✅ |
| trigger | "last time you built the report generator it made up the columns…try again" | spec-first-build | spec-first-build | ✅ |
| near-miss | "before we build in this inherited repo, make sure you understand it" | calibration-battery | calibration-battery | ✓decoy |
| unrelated | "capital of Australia?" | none | none | ✓ |

### calibration-battery — **3/3 PASS**
| Type | Prompt | Expected | Blind pick | |
|---|---|---|---|---|
| trigger | "inherited this codebase…confirm you get our conventions/data model/deploy" | calibration-battery | calibration-battery | ✅ |
| trigger (casual) | "not convinced you really get this project — sanity-check before going deeper" | calibration-battery | calibration-battery | ✅ |
| trigger | "just wrote a CLAUDE.md — how do I verify it's good and you'll follow it?" | calibration-battery | calibration-battery | ✅ |
| near-miss | "repo has no CLAUDE.md — create one so you stop forgetting" | claude-md-manager | claude-md-manager | ✓decoy |
| unrelated | "good pizza recipe for tonight?" | none | none | ✓ |

### systematic-debugging → debugging-discipline — **was 0/3 FAIL ⚠️ → now 2/3 after fix**
| Type | Prompt | Expected | As-evaluated (old name) | After rename + new description | |
|---|---|---|---|---|---|
| trigger | "build passed, now fails after my edit: 'Cannot find module ./utils'" | debugging-discipline | **superpowers:systematic-debugging** ❌ | **superpowers:systematic-debugging** ❌ (near-duplicate; unfixable by description) | ❌ still lost |
| trigger (casual) | "stuck on this login bug 2 hrs, keep trying the same fix" | debugging-discipline | **gsd-debug** ❌ | **debugging-discipline** ✅ | ✅ fixed |
| trigger | "works locally but in prod the page spins forever" | debugging-discipline | **gsd-debug** ❌ | **debugging-discipline** ✅ | ✅ fixed |
| near-miss | "nothing's broken but before launch is it robust enough to fail gracefully" | production-readiness-review | production-readiness-review ✓decoy | production-readiness-review ✓decoy | ✓decoy |
| unrelated | "write a haiku about the ocean" | none | none ✓ | _(not re-run; unaffected)_ | ✓ |

> _Rerun caveat:_ on the two recovered cases the routers chose the vibekit debugging skill (their reasoning quotes the new "stuck-loop / lightweight / active-failure" wedge) but a couple still typed the stale literal `vibekit-code:systematic-debugging` from memory — the **choice** is correct, the label is lagging. The remaining DBG-1 loss is to `superpowers:systematic-debugging`, a skill scoped almost identically ("any bug, before proposing fixes"); two installed debuggers will tie on a plain "build broke," and no description edit reliably breaks that tie.

### production-readiness-review — **3/3 PASS**
| Type | Prompt | Expected | Blind pick | |
|---|---|---|---|---|
| trigger | "AI-built in a weekend, about to launch to real users — is it ready?" | production-readiness-review | production-readiness-review | ✅ |
| trigger (casual) | "going live with my SaaS — worried each customer's data is isolated" | production-readiness-review | production-readiness-review | ✅ |
| trigger | "can you review this before I ship it to production?" | production-readiness-review | production-readiness-review | ✅ |
| near-miss | "deploying an unattended Slack bot — need a runbook of what it can do + owner" | agent-sop-generator | agent-sop-generator | ✓decoy |
| unrelated | "translate 'good morning' to Japanese" | none | none | ✓ |

### agent-sop-generator — **3/3 PASS**
| Type | Prompt | Expected | Blind pick | |
|---|---|---|---|---|
| trigger | "scheduled automation going live — one-page spec of its permissions + owner" | agent-sop-generator | agent-sop-generator | ✅ |
| trigger (casual) | "five bots running, no record of what any is allowed to touch. Help." | agent-sop-generator | agent-sop-generator | ✅ |
| trigger | "how do I write a runbook for this integration agent before handoff?" | agent-sop-generator | agent-sop-generator | ✅ |
| near-miss | "write the doc telling the AI how our codebase is structured + conventions" | claude-md-manager | claude-md-manager | ✓decoy |
| unrelated | "18% tip on $64?" | none | none | ✓ |

### claude-md-manager — **3/3 PASS**
| Type | Prompt | Expected | Blind pick | |
|---|---|---|---|---|
| trigger | "repo has no CLAUDE.md — create one (stack, architecture, conventions)" | claude-md-manager | claude-md-manager | ✅ |
| trigger (casual) | "AI keeps making the same mistake, ignores our patterns no matter what" | claude-md-manager | claude-md-manager | ✅ |
| trigger | "standardized a new error-handling pattern — how to make Claude remember it" | claude-md-manager | claude-md-manager | ✅ |
| near-miss | "update CLAUDE.md with the learnings from this session" | claude-md-management:revise-claude-md | claude-md-management:revise-claude-md | ✓decoy |
| unrelated | "recommend a sci-fi novel" | none | none | ✓ |

### chief-of-staff-plays — **3/3 PASS**
| Type | Prompt | Expected | Blind pick | |
|---|---|---|---|---|
| trigger | "triage my inbox — what needs a reply" | chief-of-staff-plays | chief-of-staff-plays | ✅ |
| trigger (casual) | "what's on my plate today? give me the rundown" | chief-of-staff-plays | chief-of-staff-plays | ✅ |
| trigger | "meeting with Sarah at Acme tomorrow — prep me" | chief-of-staff-plays | chief-of-staff-plays | ✅ |
| near-miss | "deep, fact-checked research report on EV battery supply chain w/ sources" | deep-research | deep-research | ✓decoy |
| unrelated | "convert 10 km to miles" | none | none | ✓ |

**Decoy result: 7/7 near-misses routed to the correct neighbor, 7/7 unrelated → none. Zero false positives — unchanged by the fix.**

Notably, CMM-4 ("update CLAUDE.md with this session's learnings") correctly went to the official `claude-md-management:revise-claude-md` rather than to your `claude-md-manager` — proof your CLAUDE.md skill does **not** over-grab adjacent doc-update work.

---

## 4. Behavior conformance (does output match the SKILL.md?)

Each skill was actually invoked via the Skill tool on a self-contained input. All 7 produced the promised artifact:

| Skill | Promised structure | Conformed? |
|---|---|---|
| spec-first-build | Goal / Inputs / Constraints / User stories / Data model / API / Output / Success / Failure / Out-of-scope / Open assumptions + `[ASSUMPTION]` markers | ✅ all sections, assumptions collected for confirmation |
| calibration-battery | 5-test scorecard table + Verdict | ✅ exact table + per-miss CLAUDE.md fix per row |
| debugging-discipline (was systematic-debugging) | state gap → ranked causes → test top → result → root cause + CLAUDE.md rule | ✅ full method, concluded a specific root cause — body unchanged by the rename |
| production-readiness-review | Verdict / Critical / Important / Minor / What's-already-solid; "what happens if it fails"; read/write line | ✅ caught all 4 planted vulns, ranked by blast radius |
| agent-sop-generator | 9 fields, one-page, `UNKNOWN` rule, human-approval gate on irreversible actions | ✅ flagged auto-sent POs as a required human gate |
| claude-md-manager | 4 sections (overview / architecture / conventions / current state) | ✅ + flagged inferred items as `TODO(owner)` |
| chief-of-staff-plays | triage table (Sender·Subject·Category·urgency) + drafts + note missing sources | ✅ correct play, drafts-only, noted missing connectors |

All skills respected their own guardrails (read/write line, `[ASSUMPTION]`/`UNKNOWN` markers, drafts-only). Behavior quality is excellent across the board.

---

## 5. Scorecard

| Skill | Trigger hits | False positive on decoys? | Collided / stolen by | Behavior matches SKILL.md? | Fix |
|---|---|---|---|---|---|
| spec-first-build | 3/3 | No | — | Yes | None |
| calibration-battery | 3/3 | No | — | Yes | None |
| **debugging-discipline** (was `systematic-debugging`) | 0/3 → **2/3** | No | ~~`gsd-debug` ×2~~ resolved; `superpowers:systematic-debugging` ×1 residual | Yes | ✅ **Rename + differentiate — applied** |
| production-readiness-review | 3/3 | No | — | Yes | None |
| agent-sop-generator | 3/3 | No | — | Yes | None |
| claude-md-manager | 3/3 | No | — | Yes | None |
| chief-of-staff-plays | 3/3 | No | — | Yes | None |

---

## 6. Weakest skill & the fix that was applied

### `vibekit-code:systematic-debugging` — the only failure (now `debugging-discipline`)

The description was genuinely strong (it already named "stuck on the same fix", "works locally fails in prod", the stuck-loop trigger). It lost anyway for two structural reasons:

1. **Identical name collision.** `superpowers:systematic-debugging` had the *exact same name*. A blind router cannot prefer yours on merit when two candidates both read `systematic-debugging`. **No description edit can reliably win this** — only a rename can.
2. **`gsd-debug` out-authoritied it** on the two cases that were supposed to be your wedge ("stuck for hours", "fails in prod"), because its phrase _"persistent state across context resets"_ read as more capable for a long, stuck problem.

#### Fix A — Rename (applied) ✅
Renamed `systematic-debugging` → **`debugging-discipline`** (user-facing invocation is now `/vibekit-code:debugging-discipline`). Directory renamed, `name:` frontmatter and body title updated, and the cross-reference in `production-readiness-review` + the README updated to match. This removed the identical-name collision with the superpowers skill.

#### Fix B — Differentiate the description (applied) ✅

**Old opening:**
> `Debug a problem methodically instead of flailing — likeliest cause first, one change at a time, and a hard escalation trigger when going in circles.`

**New opening (shipped):**
> `A lightweight, self-contained debugging discipline for everyday "it's broken" — likeliest cause first, one change at a time, a hard stuck-loop trigger when going in circles. Needs no planning workflow, session-state files, or extra tooling; it's the default for a normal single-session bug.`

**Added as the final sentence** (after the existing production-readiness-review boundary clause):
> `Prefer this over heavier stateful or workflow-based debug processes for an ordinary bug in any project; reach for those only when a formal multi-session debugging workflow is already in use.`

This gave the router an explicit reason to choose it over `gsd-debug` ("lightweight / default / no workflow needed") and a concrete wedge versus the generic superpowers entry.

#### Outcome
Focused rerun (3 trigger cases + the decoy, with the new name/description against the same rivals): **0/3 → 2/3.** Both `gsd-debug` losses recovered; the decoy boundary held. The one residual loss is to `superpowers:systematic-debugging` — a near-duplicate skill with effectively the same scope, which is a coexistence/redundancy issue, not something a description can resolve. If you ever want to win that too, the only real lever is to narrow `debugging-discipline` into a distinct niche (e.g. position it explicitly as the vibe-coded / AI-build debugging companion to match the rest of the suite) or to not have both debuggers installed.

### The other 6 skills — no edits made
They triggered 3/3, produced zero false positives, correctly ceded every near-miss (including correctly *losing* CMM-4 to the official `revise-claude-md`, proving they don't over-grab), and every behavior output conformed to its SKILL.md. They are well-calibrated as-is and were left unchanged.

---

## Changes shipped

- Renamed skill: `systematic-debugging` → `debugging-discipline` (SKILL.md frontmatter, body title, directory).
- Rewrote its description (wedge + boundary clauses above).
- Updated cross-reference in `production-readiness-review/SKILL.md`.
- Updated `README.md` (skill list + example invocation).
- Bumped version `1.0.0 → 1.1.0` in `plugins/vibekit-code/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` so users receive the update.
- Applied identically in the working repo and the locally-installed marketplace copy; pushing the repo publishes it to users (run `/reload-plugins` to pick it up locally).
