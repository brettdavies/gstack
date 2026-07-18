---
title: "Fit verdict: orchestrator-handoff → garrytan/gstack"
created: 2026-07-17
execution: knowledge-work
status: verdict delivered; direction is fork-first build (/orchestrate v1 built, /implement planned in 003)
skill_under_review: ~/dev/agent-skills/orchestrator-handoff
grounding: .context/repo-recon/profile.md · docs/designs/SELF_LEARNING_V0.md
related: 2026-07-17-003-feat-implement-worker-plan.md (the /implement implementation plan)
---

# Fit verdict: `orchestrator-handoff` → garrytan/gstack

## Verdict

**`go — building in the fork now.`** `orchestrator-handoff` runs daily and bulletproof in the source repos, so the
orchestration macro is proven, not speculative. Ported to gstack it maps onto shipped skills for review (`/review` +
`/health`) and learnings (`/learn` + `/retro`); the one binding with no gstack equivalent is the worker atom
(`ce-work`), which is the whole unlock.

**Direction (current): build in `brettdavies/gstack`, not upstream.** The `upstream` remote is unlinked and PRs target
the fork; upstream engagement to garrytan is deferred (the analysis below stays the eventual pitch material, and the
#2276 / supersession section describes that deferred path). `/orchestrate` (the R6 "Execution Studio" orchestrator) is
built as v1 and held for review; `/implement` (the R5 "BUILD" worker atom) is planned in
`2026-07-17-003-feat-implement-worker-plan.md`.

**Lane correction.** `/orchestrate` is **R6** (the swarm orchestrator), not R5. The **R5** piece is the worker atom
(`/implement`). The "driver" that actually runs a handoff is **proven prose** — a human-booted session following the
handoff's operating principles — not an unbuilt gap. v1 uses that human-booted-driver surface; a nested-subagent surface
(creator → driver subagent → worker subagents) that removes the human boot step is v2.

The thing brought across is the **macro** — orchestrating an approved plan into layered parallel dispatch plus always-on
learnings capture — **not the specific tool bindings.** Every compound-engineering dependency remaps onto a gstack-native
equivalent. This is a rewrite, not a copy.

## What the skill is

`orchestrator-handoff` runs in a planning session after a plan exists and emits two artifacts: (1) a local-only handoff
document (`.context/handoffs/<date>-<slug>.md`) carrying a per-unit dispatch map (exact goal text, model tier, base-SHA
verification, coding guardrails, evidence contract), dependency layers with parallel-safety notes, review-cadence
milestones, and an always-on compounding pipeline; and (2) a short paste-able "goal" that boots a brand-new orchestrator
session. Division of labor: workers implement units via the compound-engineering `ce-work` skill in return-to-caller
mode; the orchestrator owns integration, commits, reviews, PR, CI, and fires a non-blocking learnings-compounding
subagent after each accepted unit.

It is already **progressive-disclosure-shaped**: a lean ~11.6KB `SKILL.md` + five `references/*.md` + a deterministic
`scripts/scan-plan.py` + `templates/` + `evals/`. That architecture is exactly what `v2_PLAN.md` wants every gstack
skill to become.

## The pivotal finding: gstack already designed this

`docs/designs/SELF_LEARNING_V0.md` — authored via gstack's own `/office-hours` + `/plan-ceo-review` + `/plan-eng-review`
— makes three things explicit:

1. **The North Star is `/autoship`** (Release 5): "A full engineering team in one command. Describe a feature, approve
   the plan, everything else is automatic." A resumable state machine: office-hours → autoplan (single approval gate) →
   **BUILD** → /health → /review → /qa → /ship → checkpoint archive.
2. **Release 6 "Execution Studio"** names, verbatim: "Swarm orchestration: multi-worktree parallel builds… **An
   orchestrator skill dispatches independent workstreams to parallel agents, each with its own worktree.**" That is a
   one-line description of what `orchestrator-handoff` does.
3. **The adaptation policy**, in the same doc's "Acknowledged Inspiration": the roadmap was inspired by the Compound
   Engineering project, and "**We adapted every concept to fit GStack's template system, voice, and architecture rather
   than porting directly.**"

Grounding confirms R5/R6 are **not yet shipped** (no `/autoship`, `swarm`, or `execution-studio` skill exists; the only
mentions live in design docs + CHANGELOG). The territory is open.

## #2276 classification

- **Bucket:** new skill (it does not modify an existing gstack skill), but unambiguously a **core-workflow-extension** —
  a candidate implementation of the roadmapped R5/R6 orchestrator, the North Star of gstack's self-learning arc. Not the
  "standalone tool that earns no place in the core workflow" that #2276 rejects.
- **Gate result:** flips from the recon's default `halt` toward `go`, **conditional on issue-first**. The reframe the
  recon told us to find already exists, pre-written, in gstack's own roadmap: "R6 Execution Studio."

## The dominant risk is supersession, not scope

The recon's failure-mode table ranks #2 as **superseded** (13%, e.g. #2275→#2276): the maintainer re-implements, or a
newer PR wins. That is the live threat here. Garry has already scoped `/autoship` + Execution Studio as *his* headline
feature ("this is my open source software factory. I use it every day"). An owner building his own North Star is the
textbook supersession setup. This is why the recommendation is **issue-first, framed as a prototype offer**, not a
finished-PR drop: it asks whether he wants a contribution in this lane at all before any port cost is sunk.

Secondary risk is the recon's #1 (fork-CI red + merge-queue self-withdrawal) — real but mechanical, and the Ambition
steer (upstream) accepts it.

## Fit analysis: the macro ports, the tools remap

**What ports cleanly (the durable IP — bring all of it):**

1. Plan → dependency-layered dispatch map (units, model tiers, parallel-safety, file-affinity batching).
2. Worker/orchestrator division of labor (workers implement + locally verify only; orchestrator integrates, commits,
   reviews, ships).
3. Review cadence at plan seams (never per-unit), fresh reviewers, simplify-before-review, one final whole-diff pass —
   with the `ceil(units/3)` budget guardrails.
4. Always-on, non-blocking learnings compounding after each accepted unit.
5. Launch-gate approval mechanics + plan-annotation classes (Class A additive / Class B gated). This is a **direct
   expression of gstack's User Sovereignty ethos** — AI recommends, user decides, never auto-act on a plan change
   without approval.
6. Base-SHA verify-and-reset discipline (worktree-isolation stale-base guard) — applies identically under Conductor.
7. Requirements-coverage completeness + CI-reopen discipline (no orphaned acceptance surface; gates cover later-added
   surfaces) — pure Boil-the-Ocean.

**What remaps (the tool bindings — the user's "file locations and skill dispatches"):**

| orchestrator-handoff dependency (CE / Brett stack)                               | gstack / gbrain-native target                                                                                                             | Status                                                                                          |
| -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Worker: `ce-work mode:return-to-caller` (implement + verify only, envelope)      | **No shipped equivalent.** R5 BUILD-phase worker contract is unbuilt. Nearest: `/spec`'s "spawn a Claude Code agent in a fresh worktree." | **GAP — unshipped R5** (see below)                                                              |
| Reviews: `ce-simplify-code` + `ce-code-review mode:agent`                        | `/review` (Review Army, R2 shipped) + `/health` (R3 shipped)                                                                              | shipped                                                                                         |
| Compounding: `ce-compound` + `ce-compound-refresh`                               | `/learn` + `/retro` + `learnings.jsonl`; gbrain memory ingest (`gstack-memory-ingest`, `mcp__gbrain`)                                     | shipped                                                                                         |
| Learnings store: `docs/solutions/` (symlinked repo) + worktree-race commit dance | `~/.gstack/projects/$SLUG/learnings.jsonl` (append-only, "latest-winner" per key — **race-free by design**) + gbrain                      | shipped — **the entire worktree-commit dance in `compounding-pipeline.md` becomes unnecessary** |
| Handoff doc: `.context/handoffs/<date>-<slug>.md`                                | `~/.gstack/projects/$SLUG/checkpoints/*.md` (checkpoint system, R3) or a gstack-local handoff artifact                                    | remap — gstack persists to `~/.gstack`, not `.context/`                                         |
| Memory file: `.context/memory/<date>-<slug>.md`                                  | `timeline.jsonl` + checkpoints (R3)                                                                                                       | remap                                                                                           |
| Plan input: `docs/plans/` `ce-unified-plan/v1`                                   | gstack plans from `/autoplan`, `/office-hours`, `/plan-*`, `/spec`                                                                        | remap — scanner's format assumption differs                                                     |
| `/unslop`, `uuidv7`, hardcoded `~/.claude/skills/orchestrator-handoff/…`         | gstack host-agnostic placeholders, `$HOME` not `~`, route through `_gstack-command` (#2078)                                               | remap — conventions                                                                             |
| Model tiers (Fable / Opus / Sonnet)                                              | same model family; gstack expresses tiering in its own voice                                                                              | portable                                                                                        |

Net: R1/R2/R3 being shipped means the orchestrator's review + learnings + memory legs all have native landing points
**today**. The compounding pipeline actually gets *simpler* on gstack — the append-only JSONL model retires the
shared-clone git-race machinery entirely.

## The one blocking gap: no gstack-native worker primitive

The orchestrator macro assumes a worker that "implements and locally verifies only, returns a structured envelope, never
commits/reviews/ships." gstack has no shipped skill with that contract — it is precisely the R5 BUILD phase, unbuilt. A
gstack port must resolve this first, three options:

1. **Co-propose the worker contract** as part of the same effort (the honest R5 dependency).
2. **Stand in with `/spec`'s spawn-agent-in-worktree** path as the worker, wrapping it in a return-to-caller discipline
   the orchestrator enforces.
3. **Scope the first contribution to the handoff/dispatch-map authoring half only** (the pure planning output), leaving
   execution to a follow-up once the worker primitive exists.

Option 3 is the smallest defensible first PR and the cleanest "extension of an existing workflow" (it extends
`/autoplan` → produces a dispatch artifact) without needing R5 to exist yet.

## Ethos & voice fit

- **Ethos embodiment: strong.** Boil-the-Ocean completeness (requirements-coverage table, CI-reopen, nine-element
  dispatch blocks, no shortcuts). Search-Before-Building ("search for an existing helper before writing one"; pre-assign
  shared helpers so parallel workers don't reinvent). User Sovereignty (the launch-gate + Class B approval gate *is*
  AI-recommends-user-decides). This skill already thinks the way gstack thinks.
- **Voice: needs a pass.** The references are direct and verdict-like (a good base) but written in third-person
  procedural register. A port needs Garry's first-person founder voice and real sourced numbers (a "the numbers that
  matter" table: units dispatched, wall-clock vs sequential, review passes saved). No ETHOS/promo edits — none are
  implicated here.

## The tension to name honestly

gstack's *current-quarter* thrust (`v2_PLAN.md`) is **reducing** surface — "the lightest opinionated skill pack," 28/31
skills over the token ceiling, bloat is the quotable external criticism. A new heavyweight orchestration skill runs
against that current even as it serves the North Star. Two things make the tension survivable and both belong in the
issue: the skill is already progressive-disclosure-native (thin SKILL.md + `references/`), and R5/R6 are the payoff the
whole self-learning arc was built toward. The pitch must lead with "the roadmapped North Star, built v2-native," not
"here's another skill."

## Recommendation

1. **Issue first (de-risk supersession, not scope).** File a short issue that: names R6 Execution Studio and R5
   `/autoship` BUILD from `SELF_LEARNING_V0.md`; shows the working prototype (`orchestrator-handoff`) as evidence the
   pattern is real and load-bearing; states plainly that the port would be **gstack/gbrain-native** (remap table above),
   honoring the doc's own "adapt, don't port directly" policy; and asks the one question that matters: **is this a lane
   you want a contribution to prototype, or are you building R5/R6 yourself?**
2. **Only build on a green light.** A "yes, prototype it" flips this to a real build plan (a follow-up `ce-plan` →
   `ce-work`). Silence or "I've got it" = hold; the port cost is real and supersession would waste it.
3. **If building: start with Option 3** (the dispatch-map authoring half as an extension of `/autoplan`), so the first
   PR needs no unshipped R5 worker primitive. Author host-agnostic `.tmpl`; honor landed conventions (unique `name:`
   #2201, `$HOME` #1882/#1656, quoted description #1778, `_gstack-command` #2078); `gen:skill-docs --host all` + `bun
   test`; commit `.tmpl` + Claude `SKILL.md`. Community PR shape + `Fixes #<issue>`; expect red fork eval CI (no
   secrets) and wave incorporation, not self-merge.
4. **Voice pass** before any PR: first-person, sourced numbers, mirror `/office-hours` / `/plan-ceo-review`.

## What NOT to do

- Do **not** port the CE tool bindings verbatim (`ce-work`, `ce-compound`, `docs/solutions/`, the shared-clone
  worktree-commit dance). gstack's stated policy and its shipped native substrate both reject a direct port.
- Do **not** open a code PR before the issue. #2276 + supersession make a cold new-skill PR the pre-rejected path.
- Do **not** bundle a new host, and do **not** touch `ETHOS.md` / promo / Garry's voice.
- Do **not** assume the R5 worker primitive exists — it does not; design around that or co-propose it.
</content>

</invoke>
