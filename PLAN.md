# Theory-Modeling Vertical — Plan

> **For agentic workers:** REQUIRED DISCIPLINE: Use `superRA:handoff-doc` for all `PLAN.md` / `RESULTS.md` editing. This plan edits superRA skills, workflow docs, hooks, and tests — not empirical data — so the usual `superRA:econ-data-analysis` step discipline does not apply. The per-step cycle here is **plan → edit → verify (read / grep / targeted checks) → commit**.

**Objective:** Add a first-class `superRA:theory-modeling` domain vertical for mathematical-modeling work, with strict derivation discipline, notation and assumption controls, simple numerical verification, and renderable markdown/LaTeX output.

**Methodology:** Implement the new domain skill and stage-scoped references, wire it into the superRA skill manifest and planning/integration workflow surfaces, generalize data-only wording that blocks a second vertical, expose the skill for repo-local discovery, and verify the resulting structure with harness checks plus targeted smoke coverage.

**Implementation Inventory:**

### New files
| Artifact | Purpose |
|---|---|
| `skills/theory-modeling/SKILL.md` | Main domain skill body with trigger description, stage-scoped load map, and shared intuition-first four-gate checklist (Objects & Notation / Assumptions / Derivations / Verification & Rendering) |
| `skills/theory-modeling/references/planning.md` | Planning hard gate: `Model Inventory / Assumption Map` plus verification plan |
| `skills/theory-modeling/references/integrate-drift-tests.md` | Modeling-specific Phase A guidance for symbolic and numerical drift tests |
| `skills/theory-modeling/references/integration.md` | Modeling-specific integration checklist |

### Wiring surfaces
| File | Why it changes |
|---|---|
| `skills/using-superRA/SKILL.md` | Add the new domain skill to Skill Inventory and the manifest add-on table |
| `skills/planning-workflow/SKILL.md` | Route theory/modeling work to the new planning reference and generalize second-vertical wording |
| `skills/refactor-and-integrate/SKILL.md` | Point integration-stage loading at the modeling-specific integration reference |
| `skills/handoff-doc/references/plan-anatomy.md` | Generalize data-only plan template wording so a second vertical is first-class |
| `hooks/exit-plan-mode` | Remind the orchestrator about the new planning hard gate |
| `README.md`, `skills/CATEGORIES.md`, `CLAUDE.md` | Document the vertical as implemented, not roadmap-only |
| `.agents/skills/theory-modeling` | Repo-local discovery symlink |
| `tests/check-harness-compatibility.sh` | Assert the new skill is exposed and wired |

**Output:** A working `theory-modeling` vertical that auto-loads through superRA’s existing workflow, plus updated contributor/runtime docs and passing structural compatibility checks.

**Expected Results / Hypotheses:** After implementation, a modeling prompt should route through `planning-workflow` to a `Model Inventory / Assumption Map` gate before task drafting; implementer/reviewer pairs should be able to use the shared checklist to catch skipped algebra, lazy notation, hidden or uninterpretable assumptions, and broken rendering; integration should have domain-specific drift-test and refactor guidance without adding a new workflow skill.

**Pipeline:** N/A — this is plugin-engineering work, not a multi-script empirical analysis.

**Sensitivity Analysis:** N/A for this task shape. The analogue is targeted verification: wiring checks, discovery exposure, and one smoke path for planning/routing semantics.

---

## Workflow Status

- [x] **Plan approved** — researcher signed off on the direction and the initial scope for v1
- [x] **Execution complete** — all tasks `APPROVED`, verification checks pass
- [ ] **Drift tests created** — integration-phase protection added and passing on baseline
- [x] **Refactored** — integration reviewer `APPROVED` *(re-approved 2026-04-24 after Task 7 domain-neutral cleanup of workflow/utility skills)*
- [ ] **Docs finalized** — `RESULTS.md` matured, project docs audited, doc-reviewer `APPROVED`
- [ ] **Merged** — branch merged to main or PR opened

## Decisions

> **User decision (2026-04-22):** Use the skill name `theory-modeling`.
> **Question asked:** Which naming direction should the new vertical use?
> **Rationale:** Aligns with the repo roadmap wording while still allowing the v1 body to focus on mathematical modeling.

> **User decision (2026-04-22):** V1 includes derivation discipline **plus simple numerical verification**.
> **Question asked:** What should the first wired-in version cover?
> **Rationale:** Core derivation discipline is the priority, but numerical/special-case checks are part of the trust model for this vertical.

> **User decision (2026-04-22):** Ban lazy placeholder notation, but allow genuinely conventional notation when its meaning is explicit and intuitive.
> **Question asked:** How strict should the notation rule be in v1?
> **Rationale:** The vertical should reject arbitrary `A/B/C/D` or `T1/T2` labels while still permitting field-standard notation such as `r` for an interest rate.

> **User decision (2026-04-22):** Skip Phase A drift-test creation for this branch and proceed directly to Phase B integration work.
> **Question asked:** Which key results should be protected with drift tests before integration?

> **User decision (2026-04-22):** Use `main` as the integration base.
> **Question asked:** Is `origin/main` the correct base for Phase B integration, or did this branch split from a different base?

> **User decision (2026-04-23):** Add Task 4 covering both (a) a strengthened notation-ordering check and (b) an explicit mechanism for updating the PLAN.md Notation Conventions table during implementation.
> **Question asked:** Scope of the new notation-discipline task — ordering check only, update mechanism only, or both?
> **Rationale:** The existing `Define` §"defined before first use" check is weak on narrative ordering and silent on how implementers should evolve canonical notation once it is in `PLAN.md`. Covering both closes the gap and keeps the Notation Conventions table load-bearing across the whole workflow, not just at planning.

> **User decision (2026-04-23):** Rebase the branch onto current `main` (HEAD `b6e0640`), dropping the 34 unrelated objective-first task/step-semantics refactor commits and keeping only the 17 theory-modeling commits (`f761c26..16dcfe7` on the pre-rebase tree).
> **Question asked:** Reorient this branch onto current main to drop the irrelevant objective-first commits — rebase or merge?
> **Rationale:** The objective-first refactor work sat on the same branch but is a separate initiative; the user wants the theory-modeling work to stand alone on current main. Semantic-merge resolved four conflict stops during rebase (`README.md` principle #5 wording, `plan-anatomy.md` Phase B upstream-intent synthesis, `planning-workflow/SKILL.md` Remember-list synthesis, and `agents/implementer.md` with its mirrored `.codex/agents/superra_implementer.toml`) — base intent preserved; theory-modeling generalizations kept; objective-first-specific phrasings dropped. Derived artifacts (`direct-mode-*.md`) regenerated from the rebased agent sources. The earlier Phase B integration reviews on Tasks 2 & 3 flagged scope creep from the now-dropped commits; those findings are structurally resolved and the stale review-notes blockquotes were removed with integration status reset to the pre-integration default so Phase B re-runs against the new rebased base.

> **User decision (2026-04-23):** Restructure the `theory-modeling` skill body around intuition/interpretability as the through-line. (a) Rewrite the Iron Law to name stated intuition alongside defined objects and stated assumptions. (b) Drop the `Define-Derive-Validate` name entirely in favor of four gates organized around the reader's trust chain: **Objects & Notation**, **Assumptions**, **Derivations**, **Verification & Rendering**. (c) Make assumption synthesis ("prefer one stronger interpretable primitive over scattered weak restrictions") a `[BLOCKING]` gate with a reviewer judgement margin — flag only when a clearly cleaner synthesis is available. Add Tasks 5 (SKILL.md restructure) and 6 (propagation to `references/planning.md` + `references/integration.md`); roll back the `Refactored` milestone so Phase B integration re-runs on the restructured skill. Tasks 1–4 remain APPROVED — their work (skill scaffolding, wiring, verification, notation narrative-ordering and atomic Notation-Conventions updates) is preserved under the new structure, not redone.
> **Question asked:** (1) Drop `Define-Derive-Validate` entirely for a four-gate structure, or keep D-D-V and push intuition into it more aggressively? (2) Is assumption synthesis a `[BLOCKING]` gate or `[ADVISORY]`?
> **Rationale:** The researcher's original brief named intuition and interpretability as the top requirement ("most important"), with notation discipline and stepwise derivation downstream of that. The current shared checklist exercises those gates mechanically and leaves assumption synthesis out entirely. Full structural replacement is cleaner than bolting intuition onto a frame that was originally a mechanical mirror of `describe-analyze-validate` from the data-analysis vertical. Task 4's rules (notation narrative ordering, `PLAN.md` Notation Conventions as authoritative living record, atomic inline-edit of the table) compose cleanly under the new **Objects & Notation** and **Documentation and handoff** gates. Boxes unchecked: project-level `Refactored` only — Phase B re-runs after Task 6 lands; per-task `Review status` / `Integration status` on Tasks 1–4 remain APPROVED because their outputs are preserved, not reworked.

> **User decision (2026-04-23):** Proceed with integration — dispatch `superRA:integration-workflow` to run Phases A–D (drift tests skipped per the 2026-04-22 decision, iterative sync+refactor re-runs Phase B against current `main` on the restructured skill, then docs finalization, then merge/PR).
> **Question asked:** Completion menu after Tasks 5 and 6 APPROVED — proceed with integration, change the plan, keep branch as-is, or discard?
> **Rationale:** Tasks 5 and 6 rewrote the skill body (four gates, new Iron Law, five new `[BLOCKING]` items) and propagated the framing into the planning and integration references; the `Refactored` milestone was rolled back by the earlier 2026-04-23 restructure decision so Phase B re-runs on the restructured skill before merge.

> **User decision (2026-04-24):** Open the PR before the Document phase — skip the doc-writer maturation pass for now; reviewers can evaluate the branch against PLAN.md and the in-tree RESULTS.md draft.
> **Question asked:** Where should RESULTS.md land permanently under `docs/plans/`, or should the Document phase be deferred?
> **Rationale:** User wants PR review to start in parallel; RESULTS.md maturation and relocation can land as a follow-up commit on the PR branch if reviewers need the permanent record before merge.

> **User decision (2026-04-24):** Add Task 7 — make workflow and utility skills domain-neutral. Drop redundant per-domain "and read references/X.md" wording from `planning-workflow`, `refactor-and-integrate`, `result-protection`, `result-protection/references/drift-test-quality.md`, and `agents/reviewer.md`; the active domain skill's own stage-load table is the authoritative map for which references load at which stage. Examples drawn from a particular domain stay (they illustrate domain-neutral concepts). Domain-skill discovery/inventory entries in `using-superRA/SKILL.md` and the trigger column of `planning-workflow` Phase 1's table also stay (they are routing, not redundant load directives). Roll back the project-level `Refactored` milestone so Phase B integration re-runs on the cleaned-up skills; Tasks 1–6 remain APPROVED — their content is preserved, only cross-skill load directives are pruned.
> **Question asked:** Workflow skills still describe what to load for specific domains even though each domain skill carries its own stage-load table — should the workflow be made domain-neutral?
> **Rationale:** Restating per-domain load instructions in workflow/utility skills duplicates the authoritative table in the domain skill and creates drift risk when a new vertical lands or an existing vertical's reference layout changes. Per `CLAUDE.md` §"Teach the Protocol, Don't Prescribe Each Action" and the DRY test, the workflow should route to the domain skill and trust its load map; only examples drawn from a specific domain to illustrate a domain-neutral point are kept.

> **User decision (2026-04-24):** Force-push with `--force-with-lease` to update existing DRAFT PR #17.
> **Question asked:** Remote tracks pre-rebase history (HEAD `16dcfe7`) with DRAFT PR #17 open; update in place or open a new PR?
> **Rationale:** The 2026-04-23 rebase-onto-main decision already rewrote the branch intentionally; updating #17 keeps the PR's existing draft discussion and is the standard flow for rebase-updated branches.

## Project Conventions

Walked at planning time (2026-04-22). Re-walk on-demand only.

### Repo root
- `/CLAUDE.md` (HEAD at `b6e0640`): Contributor-facing authority for superRA changes. New verticals must be added as domain skills, not workflow forks; shared checklists are load-bearing; `skill-creator` discipline applies when editing `skills/*/SKILL.md`; README, categories, and manifest surfaces must stay in sync; Codex discovery uses `.agents/skills/` symlinks and generated `.codex/agents/` files remain derived artifacts.
- `/AGENTS.md`: symlink to `/CLAUDE.md`; same contributor guidance for AGENTS-aware harnesses.
- `/README.md` (HEAD at `b6e0640`): User-facing overview of the plan/implement/integrate workflow, current domain skills, utility skills, installation, and hooks. Domain skill additions need matching README updates, especially the Domain Skills table and roadmap wording.

### Not walked (not needed for this plan)
- `docs/plans/` beyond targeted reference examples, `tests/claude-code/`, and harness-specific install docs outside the repo root summaries — out of scope unless verification shows a drift.

---

### Task 1: Create the `theory-modeling` domain skill and its stage-scoped references
**Depends on:** *(none)*
**Review status:** APPROVED
**Integration status:** APPROVED (post-sync re-review 2026-04-24)
**Final diff self-check:** `git diff 886fda8..HEAD -- skills/theory-modeling/SKILL.md skills/theory-modeling/references/`; surviving hunks are the new-file additions of `SKILL.md`, `references/planning.md`, `references/integrate-drift-tests.md`, `references/integration.md` (task objective) — the post-sync cross-skill pointer rewrites in the two `references/` files are recorded under this task's Sync impact; no suspicious hunks.

**Script:** `skills/theory-modeling/SKILL.md`, `skills/theory-modeling/references/*.md`
**Input:** Existing domain-skill pattern in `skills/econ-data-analysis/` plus the approved v1 requirements in this plan’s Decisions section
**Output:** New `theory-modeling` skill directory with a main checklist and the planning / drift-test / integration references

- [x] Draft `SKILL.md` with trigger language, stage-scoped references, a short iron-law equivalent, and a shared `Define–Derive–Validate` checklist using `[BLOCKING]` / `[ADVISORY]`.
- [x] Draft `references/planning.md` with the `Model Inventory / Assumption Map` hard gate and the result-verification planning rules.
- [x] Draft `references/integrate-drift-tests.md` and `references/integration.md` so integration has modeling-specific guardrails for symbolic identities, comparative statics, and simple numerical checks.
- [x] Sanity-read the new vertical for internal consistency, then update `RESULTS.md` Task 1 with the new checklist and any open caveats.

### Task 2: Wire the new vertical into runtime surfaces, docs, and discovery
**Depends on:** *(none)*
**Review status:** APPROVED
**Integration status:** APPROVED (post-sync re-review 2026-04-24)
**Final diff self-check:** `git diff 886fda8..HEAD -- skills/using-superRA/SKILL.md skills/planning-workflow/SKILL.md skills/refactor-and-integrate/SKILL.md skills/handoff-doc/references/plan-anatomy.md hooks/exit-plan-mode README.md skills/CATEGORIES.md CLAUDE.md .agents/skills/theory-modeling tests/check-harness-compatibility.sh agents/ .codex/ skills/using-superRA/references/direct-mode-*.md`; surviving hunks are (a) theory-modeling manifest / routing / docs / hook / test additions (task objective), (b) the two flagship-discipline row rewrites in `README.md:67` and `skills/CATEGORIES.md:25` (four-gate framing), (c) post-sync short-wording adoption in `agents/*.md`, `.codex/agents/*.toml`, and direct-mode role references (Sync impact); no suspicious hunks.

**Script:** Existing workflow/docs/hook/test files named in the Implementation Inventory, plus `.agents/skills/theory-modeling`
**Input:** The approved skill name, the new file layout from Task 1, and the current runtime/docs wording in the repo
**Output:** Updated manifest/routing/docs/hook/test surfaces that expose `theory-modeling` as an implemented vertical and remove the remaining single-vertical assumptions that would block it

- [x] Update `skills/using-superRA/SKILL.md`, `skills/planning-workflow/SKILL.md`, and `skills/refactor-and-integrate/SKILL.md` so the new vertical loads at planning, drift-test, and integration time.
- [x] Generalize `skills/handoff-doc/references/plan-anatomy.md` and `hooks/exit-plan-mode` so planning guidance and reminders support both data-analysis and theory-modeling hard gates.
- [x] Update `README.md`, `skills/CATEGORIES.md`, and `CLAUDE.md` so the vertical is documented as implemented rather than roadmap-only.
- [x] Add `.agents/skills/theory-modeling` and extend `tests/check-harness-compatibility.sh` with the new discovery/wiring assertions.

### Task 3: Verify the new vertical end to end and reconcile any drift
**Depends on:** Task 1, Task 2
**Review status:** APPROVED
**Integration status:** APPROVED (post-sync re-review 2026-04-24)
**Final diff self-check:** `git diff 886fda8..HEAD -- tests/check-harness-compatibility.sh`; surviving hunks are the theory-modeling discovery/wiring assertions (task objective), post-sync retargeted at `refactor-and-integrate/SKILL.md` with the two assertions targeting the deleted references dropped (Sync impact); branch-side edits to the deleted `refactor-and-integrate/references/merge-quality.md` honored as upstream-delete (no restoration); no suspicious hunks.

**Script:** Verification commands and any touched files needed to resolve resulting failures
**Input:** Completed outputs from Tasks 1 and 2
**Output:** Passing targeted checks, updated handoff docs, and a final repo state ready for integration

- [x] Run `tests/check-harness-compatibility.sh` and fix any failures surfaced by the first verification round.
- [x] Run a targeted smoke check on the new routing and repo-local discovery surfaces.
- [x] If any agent or doc examples were touched, run the matching generated-artifact check and reconcile drift.
- [x] Generalize the Tier 3 / Never-list wording in `skills/refactor-and-integrate/references/merge-quality.md` so theory/modeling is first-class in the blocking checklist text.
- [x] Tighten the merge-quality assertion in `tests/check-harness-compatibility.sh` so it verifies the generalized wording or explicitly fails on the old data-only phrases.
- [x] Re-run the structural verification checks, then replace `RESULTS.md` Task 3 with the final verification outcome, remaining risks, and the exact checks that passed.

### Task 4: Tighten notation discipline — strengthen the ordering check and add an explicit Notation Conventions update mechanism
**Depends on:** Task 1
**Review status:** APPROVED
**Integration status:** APPROVED

**Script:** `skills/theory-modeling/SKILL.md`, `skills/theory-modeling/references/planning.md`
**Input:** Existing Define-Derive-Validate checklist (SKILL.md) and the Model Inventory / Assumption Map template (references/planning.md)
**Output:** A stronger `Define` block that enforces narrative-order introduction of symbols and names PLAN.md's Notation Conventions table as the authoritative cross-task source, plus an explicit rule — in both the skill body and the planning reference — that implementers inline-edit the Notation Conventions table BEFORE using any newly introduced symbol in algebra, committed atomically with the work.

- [x] Rewrite the existing `Define` §`[BLOCKING]` *"Every symbol is defined before first use"* item in `skills/theory-modeling/SKILL.md` to require (a) narrative-order introduction — a symbol may not appear in any derivation, equation, proof step, or verification before the paragraph/table that introduces it; and (b) PLAN.md Notation Conventions as the authoritative source for any symbol reused across tasks.
- [x] Add a new `[BLOCKING]` item in `Documentation and handoff` stating: when implementation introduces a symbol not yet in PLAN.md's Notation Conventions table, update the table via inline-edit BEFORE using the symbol in algebra, and commit the PLAN.md edit atomically with the derivation work. References `superRA:handoff-doc` inline-edit discipline rather than restating it.
- [x] Mirror the update mechanism in `skills/theory-modeling/references/planning.md` under "Principles" — the planning reference now flags the Notation Conventions table as a living record, not a one-time planning artifact.
- [x] Add one row to the Common Rationalizations table in `SKILL.md` capturing the new failure mode ("I'll update the Notation Conventions table after the derivation is clean." → "Late notation updates mean the derivation was written against undefined symbols; update the table first, then derive.").
- [x] Sanity-read the edits for internal consistency with the Iron Law and existing checklist items; updated `RESULTS.md` Task 4 with the final checklist wording and any open caveats.

### Task 5: Restructure the `theory-modeling` SKILL body around intuition/interpretability as the through-line
**Depends on:** Task 1, Task 4
**Review status:** APPROVED
**Integration status:** APPROVED (post-sync integration review 2026-04-24)
**Final diff self-check:** `git diff 886fda8..HEAD -- skills/theory-modeling/SKILL.md`; `SKILL.md` is new-file in this governing range — Task 1 scaffolding and Task 5 four-gate restructure are collapsed into a single additive hunk (task objective); no surviving hunks outside the new file; no suspicious hunks.

**Script:** `skills/theory-modeling/SKILL.md`
**Input:** Existing SKILL.md body (Iron Law + `Define-Derive-Validate` checklist + Common Rationalizations); researcher feedback that intuition and interpretability must be the organizing spine, that the D-D-V framing mechanically mirrors the data-analysis vertical, and that assumption synthesis is currently absent
**Output:** Restructured SKILL.md whose Iron Law, shared checklist, and Common Rationalizations put intuition and interpretability at the center of every modeling move, with the four-gate structure replacing D-D-V and assumption synthesis as a `[BLOCKING]` gate

- [x] Rewrite the Iron Law to name stated intuition alongside defined objects and interpretable assumptions. Target wording: "NO MANIPULATION WITHOUT DEFINED OBJECTS, INTERPRETABLE ASSUMPTIONS, AND STATED INTUITION" with the three-line expansion: *"Every symbol has a meaning. Every assumption has a plain-language interpretation a researcher can defend. Every non-trivial move has a one-sentence reason."* Rewrite the "No exceptions" bullets to match.
- [x] Replace the `Define-Derive-Validate` section with a four-gate structure — **Objects & Notation**, **Assumptions**, **Derivations**, **Verification & Rendering** — organized around the reader's trust chain. Preserve every existing `[BLOCKING]` / `[ADVISORY]` item that remains load-bearing (notation discipline, assumption-on-primitives rule, domain/unit/sign requirements, solution-concept naming, one-move-per-step rule, case-split discipline, comparative-statics discipline, symbol-consistency rule, existence/uniqueness-requires-argument rule, verification modes, numerical-parameter requirement, assumption-map check-back, CAS transcription rule, rendering clarity) and relocate each under the gate where it belongs. Preserve Task 4's narrative-ordering rule and the atomic Notation-Conventions-table-update rule intact under **Objects & Notation** and **Documentation and handoff** respectively.
- [x] Add the following new `[BLOCKING]` items under the appropriate gate:
  - **Objects & Notation:** every new symbol introduced during implementation carries a stated intuition or mnemonic (one short sentence), unless it reuses a conventional name already in `PLAN.md`'s Notation Conventions.
  - **Assumptions (interpretability):** each assumption carries a one-sentence plain-language interpretation a researcher can defend (e.g., "risk aversion bounded so the value function is finite"); assumptions stated only as math restrictions without economic interpretation are REVISE.
  - **Assumptions (synthesis):** when multiple scattered assumptions can be replaced by a single stronger primitive assumption with a cleaner interpretation, prefer the synthesis and record the trade. Reviewer applies a judgement margin — flag only when a clearly cleaner synthesis is available.
  - **Derivations (reason per move):** each non-trivial step carries both the technical rule (envelope theorem, market clearing, …) and a one-sentence reason for invoking it; mechanical rule-labels without a reason are REVISE.
  - **Verification (economic interpretation):** special and limiting cases are interpreted economically, not just numerically confirmed (e.g., "at `β → 0` the policy reduces to the myopic rule, which matches the one-period benchmark").
- [x] Add rows to the Common Rationalizations table for the four missing intuition-failure modes: "The intuition is obvious."; "I'll add interpretation after the algebra is clean."; "Weaker assumptions are always safer."; "This assumption is technical, not economic." Pair each with the corresponding reality statement in the same style as existing rows.
- [x] Update every internal reference inside SKILL.md that points at "Define-Derive-Validate" (reviewer-protocol section pointer, stage-scoped subsection language, Key References list) to point at the new four-gate section name.
- [x] Sanity-read the restructured skill for internal consistency with the new Iron Law, Task 4's notation/update rules, and the stage-scoped reference bodies; update `RESULTS.md` Task 5 with the final checklist wording and any open caveats.

### Task 6: Propagate intuition/interpretability gates into `references/planning.md` and touch up `references/integration.md`
**Depends on:** Task 5
**Review status:** APPROVED
**Integration status:** APPROVED (post-sync integration review 2026-04-24)
**Final diff self-check:** `git diff 886fda8..HEAD -- skills/theory-modeling/references/planning.md skills/theory-modeling/references/integration.md`; both files are new-file in this governing range — Task 1 scaffolding and Task 6 intuition/interpretability propagation are collapsed into single additive hunks per file (task objective), with the post-sync pointer / verdict-protocol rewrites in `integration.md` already folded in (Sync impact); no suspicious hunks.

**Script:** `skills/theory-modeling/references/planning.md`, `skills/theory-modeling/references/integration.md`
**Input:** Restructured SKILL.md from Task 5, existing Model Inventory / Assumption Map template, existing integration-stage checklist
**Output:** Planning template and integration checklist aligned with the intuition-first framing so planning-time inventory records intuition/interpretability up front and integration-time review verifies those artifacts survive refactor

- [x] In `references/planning.md` Model Inventory template: add an **Interpretation** column to the Assumptions table so every planned assumption records the plain-language reading at planning time; tighten the Notation Conventions section so the existing "Why this notation" column is explicitly required for non-conventional symbols rather than optional. The Interpretation column carries one concrete example row ("risk aversion bounded so the value function is finite") showing the target one-short-phrase shape for downstream implementers without prescribing specific economic content.
- [x] In `references/planning.md` §Principles: add one principle stating that interpretability of assumptions is a blocking requirement and that synthesizing scattered weak assumptions into a stronger interpretable primitive is preferred. Point at SKILL.md for the checklist; do not restate its items here.
- [x] In `references/planning.md` §Common mistakes and §Red Flags — Hard Gate Protection: add items that name the intuition-failure modes the implementer must not defer ("notation interpretation TBD", "assumption interpretation later", "scattered weak assumptions that could be synthesized").
- [x] In `references/integration.md` §"Derivation discipline preserved through refactoring": add `[BLOCKING]` items that each new-notation intuition, each assumption interpretation, and each derivation-step reason recorded in the original work survives the refactor — not silently collapsed into opaque prose or code comments.
- [x] Clean up the two remaining `Define-Derive-Validate` mentions as part of this task: `references/planning.md` §"Handoff to Implementation" now names the new through-line (intuition + interpretability + stated reason across the four gates); `references/integration.md` opening paragraph's verdict-protocol pointer now references `theory-modeling/SKILL.md ## The Four Gates`.
- [x] Sanity-read both references for internal consistency with the new SKILL.md structure and with `superRA:handoff-doc` inline-edit discipline; update `RESULTS.md` Task 6 with the final wording and any open caveats.

### Task 7: Make workflow and utility skills domain-neutral
**Depends on:** *(none)*
**Review status:** APPROVED
**Integration status:** APPROVED (integration refactor review 2026-04-24)
**Final diff self-check:** `git diff 886fda8b..HEAD` + working tree on Task 7 files; surviving-change classes — domain-neutral rewrites of five load-directive sites in `planning-workflow/SKILL.md`, one-line drops in `refactor-and-integrate/SKILL.md` and `result-protection/SKILL.md`, one-line rewrite in `drift-test-quality.md` and `agents/reviewer.md`, regenerated `direct-mode-reviewer.md` + `.codex/agents/superra_reviewer.toml`, flipped assertion in `tests/check-harness-compatibility.sh`; suspicious-hunk justifications — none beyond Task 7 bullet scope; no surviving per-domain `references/X.md` load directives in workflow/utility skills or canonical role specs (grep sweep clean); `check-harness-compatibility.sh` 42/42 PASS, Codex agent generation 6/6 PASS.

**Script:** `skills/planning-workflow/SKILL.md`, `skills/refactor-and-integrate/SKILL.md`, `skills/result-protection/SKILL.md`, `skills/result-protection/references/drift-test-quality.md`, `agents/reviewer.md` (and regenerated direct-mode / Codex artifacts)
**Input:** Current workflow/utility skill bodies that name `superRA:econ-data-analysis` / `superRA:theory-modeling` (and their `references/*.md`) in load directives, plus the domain skills' own stage-load tables (`skills/econ-data-analysis/SKILL.md` lines ~21–29, `skills/theory-modeling/SKILL.md` lines ~26–33) which are the authoritative map for which reference loads at which stage
**Output:** Workflow and utility skills route to the active domain skill without restating its per-stage load map; examples drawn from a particular domain remain where they illustrate a domain-neutral concept

- [x] In `skills/planning-workflow/SKILL.md` Phase 1: drop the "Planning reference" column from the Currently-implemented-verticals table (the trigger column stays for routing), and collapse the two parallel "If the task is data analysis: ... If the task is theory / modeling: ..." paragraphs into one domain-neutral instruction that says: stop here, load the matching domain skill, follow its planning-stage reference, and satisfy its planning hard gate before returning to Phase 2.
- [x] In `skills/planning-workflow/SKILL.md` Phase 3 (`File Structure`): drop the explicit `(see econ-data-analysis/references/notebook-format.md)` parenthetical — the domain skill's own stage-load table already routes notebook-format guidance.
- [x] In `skills/planning-workflow/SKILL.md` Phase 4 (`Step Granularity`): replace "For data analysis, that discipline is the three concurrent disciplines describe-analyze-validate (see `superRA:econ-data-analysis` main body)" with a domain-neutral sentence pointing at the active domain skill's checklist; the example step shapes that follow may stay (examples are allowed).
- [x] In `skills/planning-workflow/SKILL.md` §Remember: replace the "For data analysis, row counts logged ..." bullet and the "Domain-appropriate discipline (for data: ... for theory/modeling: ...)" bullet with a single domain-neutral bullet pointing at the active domain skill's per-step discipline.
- [x] In `skills/planning-workflow/SKILL.md` §Self-Review item 6: replace the per-domain branch ("For data analysis ... For theory/modeling ...") with a single domain-neutral question that points at the active domain skill's verification/robustness requirements; item 1 stays as-is because its parenthetical is examples, not load directives.
- [x] In `skills/refactor-and-integrate/SKILL.md`: drop the line "For data-analysis integration, also load `econ-data-analysis/references/integration.md`. For theory/modeling integration, also load `theory-modeling/references/integration.md`." — the domain skill's own stage-load table routes the integration reference at the `integration` stage.
- [x] In `skills/result-protection/SKILL.md`: drop the line "For data-analysis result protection, also load `econ-data-analysis/references/integrate-drift-tests.md` ..." — same reason; the domain skill's stage-load table routes it at the `drift-test` stage.
- [x] In `skills/result-protection/references/drift-test-quality.md` §"Tolerance calibration": replace "For data-analysis work, load `econ-data-analysis/references/integrate-drift-tests.md` section Tolerance Conventions for Econ Results." with a domain-neutral pointer to the active domain skill's `integrate-drift-tests.md` (or equivalent) for tolerance conventions specific to that domain.
- [x] In `agents/reviewer.md` "Active check for task format" bullet: replace the per-domain enumeration ("For data-analysis scripts ... For theory/modeling notes ...") with a domain-neutral instruction that defers to the active domain skill's format/rendering reference; regenerate `skills/using-superRA/references/direct-mode-reviewer.md` and `.codex/agents/superra_reviewer.toml` via `skills/codex-superra-setup/scripts/sync_codex_agents.py`.
- [x] Verify: `tests/check-harness-compatibility.sh` still passes; grep the touched files to confirm no `econ-data-analysis/references/` or `theory-modeling/references/` load directives survive in workflow/utility skill bodies (examples and inventory entries excepted).
