---
title: "Approach-plan: skill → garrytan/gstack fit analysis"
created: 2026-07-17
execution: knowledge-work
status: deferred — analysis complete; the /orchestrate + /implement build it led to is dropped (not shipped, not upstreamed)
grounding: .context/repo-recon/
---

# Approach-plan: skill → garrytan/gstack fit analysis

> **Status: DEFERRED, not shipped.** The `/orchestrate` and `/implement` work this analysis led to is not being pursued
> and is not pushed upstream. Retained as a design record only; a future revival rebuilds fresh rather than reusing the
> dropped `feat/orchestrate` branch. Current status: the `orchestrator-handoff-gstack-fit-verdict` memory.

> **Status: executed.** The skill was `orchestrator-handoff` (`~/dev/agent-skills/orchestrator-handoff`). Stages 1–3 ran
> and produced the fit verdict (`2026-07-17-002-orchestrator-handoff-gstack-fit-verdict.md`). The direction settled on a
> fork-first build: `/orchestrate` (the R6 orchestrator) is built as v1, and `/implement` (the R5 worker atom) is planned
> in `2026-07-17-003-feat-implement-worker-plan.md`. The staged process below is the record of how that analysis was run.

## ⚑ Execution grounding — read `.context/repo-recon/` FIRST (front of mind)

Every stage is grounded in the completed recon. **Before executing any stage, (re)read the relevant recon file.** Treat
`.context/repo-recon/profile.md` as the source of truth and the phase files as the detail. If the recon is more than 30
days stale (recon date 2026-07-17), refresh with `/repo-recon garrytan/gstack` before proceeding.

| Recon file                                                      | What it grounds                                                                                                  |
| --------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `.context/repo-recon/profile.md`                                | Verdict, viability, **Voice & Ethos**, Recommended Approach, Scope Boundaries, precedents — the master reference |
| `.context/repo-recon/issue-themes.md`                           | Skill-authoring precedents + the collision zone (multi-host gen pipeline)                                        |
| `.context/repo-recon/conventions.md`                            | Skill-authoring + cross-host conventions                                                                         |
| `.context/repo-recon/tests.md`                                  | Integration test surface + the `skill-docs` CI gate                                                              |
| `.context/repo-recon/community.md`                              | AI policy (explicitly welcome), wave-incorporation accept-path                                                   |
| `.context/repo-recon/git-history.md`                            | Active refactors (`_gstack-command` router #2078, resolver-suppression) = rebase risk                            |
| `.context/repo-recon/docs.md` · `settings.md` · `technology.md` | fork-CI caveat, branch/merge shape, license + deprecation gates                                                  |

**Pivotal variable:** the gate that flips the verdict `halt` ⇄ `go` is **scope classification against #2276** ("we
aren't accepting new *standalone* skills right now… revisit if it becomes part of core workflow").

## Known-now (concrete, no skill needed)

- Recon done + cached; repo-local dev loop ready (`bun install` complete) — no re-derivation.
- Ready-made checklists live in `profile.md` (Voice & Ethos, conventions, cross-host contract, Pre-Submission
  Checklist).

## Staged process (gated steps carry objective + approach, resolved once the skill is in hand)

### Stage 1 — Intake & scope classification · pivotal gate, runs first

- **Objective:** understand the skill, then bucket it — new-standalone / core-workflow-extension /
  existing-skill-improvement.
- **Approach:** read the skill at the provided local path URI (its `SKILL.md` / `.tmpl`): purpose, trigger, behavior,
  allowed tools; map onto gstack's ~23-skill taxonomy for overlap or duplication.
- **Consult:** `profile.md` §§ Contribution Viability, Scope Boundaries, Relevant Precedents (#2276); `issue-themes.md`
  § Skill-authoring precedents.
- **Gate:** standalone → `halt` / reframe (Stage 3); extension or existing-improvement → `go`.

### Stage 2 — Fit analysis · gated on Stage 1

- **Objective:** technical and philosophical fit.
- **Approach:** duplication check vs existing skills + the recon precedents; technical fit (needs `browse`? fits the
  `.tmpl` / placeholder / multi-host model? external-API deps → deprecation gate); **voice/ethos + ethos-embodiment gap
  analysis** (Boil-the-Ocean / Search-first / User-Sovereignty behavior; Garry-voice prose).
- **Consult:** `profile.md` § Voice & Ethos + § Issue Themes; `conventions.md`; `technology.md` (deprecation/license
  gate); `docs.md`.

### Stage 3 — Positioning / reframe · gated on Stages 1–2

- **Objective:** if standalone, pick the strongest "extension of an existing core workflow" framing (or decide to hold);
  if existing-improvement, skip to Stage 4.
- **Approach:** name the workflow it plugs into (from Stage 2's mapping); draft the issue-first pitch that de-risks
  #2276.
- **Consult:** `profile.md` § Recommended Approach (step 1); `community.md` (AI policy).

### Stage 4 — Integration / authoring plan · gated; likely a follow-up `ce-plan` → `ce-work`

- **Objective:** the concrete port + integrate plan.
- **Approach:** host-agnostic `.tmpl` honoring landed-PR conventions (unique `name:`, `$HOME` not `~`, quoted YAML
  description, route through `_gstack-command`); `gen:skill-docs --host all` for free cross-host; validate `bun test` +
  `skill-docs` freshness; commit `.tmpl` + Claude `SKILL.md`. Detailed enough only once the skill + bucket are known.
- **Consult:** `conventions.md`; `tests.md`; `profile.md` § Recommended Approach (steps 3–5); `git-history.md` (active
  refactors = rebase risk).

### Stage 5 — Upstream engagement · gated on the Ambition fork

- **Objective:** decide the submission path (or skip it).
- **Approach:** issue-first with the reframe; PR to `main` via `feat/…`, community PR shape, expect wave-incorporation;
  fork eval-CI red is expected (no secrets).
- **Consult:** `profile.md` § Contribution Process; `community.md`; `settings.md`; `docs.md` (fork-CI caveat).

## Forks worth confirming (your steer changes the plan)

1. **Ambition** — upstream-merge into garrytan/gstack, or just make it gstack-shaped for your own fork? The latter makes
   Stage 5 largely evaporate and #2276 stop mattering.
2. **End state** — stop at verdict + recommendation, or continue into a full implementation plan and actually port it?

## Open questions

- Skill local path URI (blocks Stage 1) — Brett to provide.
- Ambition fork (answerable now to sharpen Stage 5).
