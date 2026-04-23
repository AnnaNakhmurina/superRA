# Workflow Frontier Resolver - Results

> Mirrors PLAN.md structure. Updated after each step with key findings.
> New agents: read PLAN.md for what to do, RESULTS.md for what was found.

**Last updated:** 2026-04-23 (over-prescription audit scope added)
**Status:** Tasks 1-4 approved; Task 5 implemented and awaiting review; Tasks 6-9 pending.

---

## Design Diagnosis

The current resolver's real value added is narrow:

- It forces agents to read durable evidence before acting, instead of trusting chat context or an invented global state.
- It preserves unrelated approved work by computing the affected task frontier from changed tasks and dependencies.
- It routes work to the workflow that owns the earliest invalid layer, so phase ownership stays local.
- It enforces gates agents often bypass under pressure: logged user decisions, review approval, reproducibility/completion before integration, and integration/docs/freshness before merge or PR.

The lengthy named-state taxonomy is not the core mechanism. Agents can usually infer "what comes next" from a canonical workflow map if the docs give them the map, the evidence to inspect, the decision object to produce, and the hard gates they may not bypass.

There is also an overview placement gap. README explains the PLAN -> IMPLEMENT -> INTEGRATE cycle and adaptive/composable idea, but runtime agents may not read README. The compact operational overview should live in a loaded runtime surface such as `skills/using-superRA/SKILL.md`, with `main-agent.md` carrying the main-agent-specific resolver mechanism.

---

## Task 1: Add Runtime Workflow Overview and Resolver Value Proposition

**Status:** Approved.

### Findings
- Added `skills/using-superRA/SKILL.md` `## Runtime Workflow Map`, which gives loaded agents the PLAN -> IMPLEMENT -> INTEGRATE order without importing README-level product explanation.
- The overview states the adaptive re-entry principle: enter at the earliest invalid layer for the affected task frontier while preserving unrelated approved work.
- The resolver value proposition is now explicit: inspect durable evidence, compute the affected frontier, route to the owning workflow, and enforce non-negotiable gates.

### Files Changed
- `skills/using-superRA/SKILL.md`
- `skills/using-superRA/references/main-agent.md`

## Task 2: Replace Contingency Taxonomy with a Frontier Mechanism

**Status:** Approved.

### Findings
- Replaced the resolver's named `needs ...` outcome list with an ordered mechanism in `skills/using-superRA/references/main-agent.md`.
- Preserved the evidence contract: git state, handoff-doc presence and consistency, workflow rollups, decisions, upstream intent, task dependencies/statuses/review notes, and RESULTS.md sections.
- Preserved the decision object: affected frontier, preserved-approved tasks, invalidated milestones, next owner/layer, and required stop point.
- Kept explicit safety invariants for the predictable failure modes: no global `Current state` field, no unlogged material decisions, no clearing unrelated task statuses, no implementation advancement without review/adjudication, no integration before reproducibility/disposition, and no merge/PR before integration/docs/freshness gates.
- The clarity revision now separates diagnosis/routing from workflow-owned actions: `main-agent.md` says the resolver diagnoses and routes, while the owning workflow performs plan edits, implementation, integration, or merge work.
- The rollup wording now says checked milestones are invalid when they no longer match task evidence or required global gates; the owning workflow or plan-change protocol should uncheck them and record why.
- The plan-change boundary is explicit: material plan changes route to `planning-workflow §User Feedback and Changing Plans`, then the resolver runs again after the docs are updated.

### Files Changed
- `skills/using-superRA/references/main-agent.md`

## Task 3: Simplify Workflow Call Sites Around the Mechanism

**Status:** Approved.

### Findings
- `implementation-workflow` no longer branches on resolver state labels. It now enters only when the resolver selects implementation, review, reproducibility verification, or the completion disposition.
- `planning-workflow` points to the resolver for cross-workflow entry selection after a material plan change and leaves local plan-change mechanics in the planning skill.
- `integration-workflow` keeps the Phase A-D gate map and scopes work to the affected frontier without restating resolver selection logic.
- `agent-orchestration` still owns dispatch and review-status mechanics; a status-table phrase was tightened so it does not resemble the old resolver taxonomy.
- `handoff-doc` and `plan-anatomy.md` remain standalone sources for handoff status semantics; no change was needed there.

### Files Changed
- `skills/agent-orchestration/SKILL.md`
- `skills/planning-workflow/SKILL.md`
- `skills/implementation-workflow/SKILL.md`
- `skills/integration-workflow/SKILL.md`

## Task 4: Audit Against Adaptive-Composable Design

**Status:** Approved.

### Findings
- The modified resolver reads as a mechanism: evidence first, affected-frontier calculation, ordered owner routing, and safety invariants.
- The loaded runtime overview is in `skills/using-superRA/SKILL.md`, not only README or AGENTS.
- Ownership boundaries remain aligned with AGENTS.md: `using-superRA` owns the shared workflow map, `main-agent.md` owns the resolver, workflow skills own local gates, `agent-orchestration` owns dispatch/status mechanics, and `handoff-doc` owns document semantics.
- Design-text search no longer finds the old resolver state labels in modified resolver/call-site prose. Remaining hits are intentional non-taxonomy uses: the explicit `Current state` prohibition, local `skip` / `re-entry` wording in workflow gates and adapter/domain skills, and unrelated discard/AskUserQuestion wording.
- The next audit will add reviewer feedback specifically on clarity and the boundary between resolver routing and the change-plan protocol.
- A focused reviewer pass approved the clarity revision, including the diagnosis/routing distinction, change-plan boundary, workflow-status rollups, and task-local status preservation.

### Verification Commands
- `rg -n "needs plan repair|needs implementation|awaiting review|needs validation|Current state|state machine|skip|resume|re-entry|if .* then|under .* condition" skills/using-superRA skills/*/SKILL.md`
- `git diff --check`
- `uv run python /Users/zhiyufu/Dropbox/app_settings/dotfiles/claude/.claude/skills/.system/skill-creator/scripts/quick_validate.py <modified-skill-folder>` for `skills/using-superRA`, `skills/planning-workflow`, `skills/implementation-workflow`, `skills/integration-workflow`, and `skills/agent-orchestration`

## Task 5: Document "Teach the Protocol, Don't Prescribe Each Action" Principle

**Status:** Implemented; awaiting review.

### Findings
- Added `CLAUDE.md §Teach the Protocol, Don't Prescribe Each Action` under `## Internal Design Philosophy` with the governing test, two ordered sub-tests (DRY, necessity), anti-patterns drawn from current repo examples, and a distinction for behavior-shaping instructions that must be kept.
- Extended `## Design Audit Checklist` with a per-line test: does removing it change what the agent would *do*, or only what it would *understand*?

### Files Changed
- `CLAUDE.md`

## Task 6: Audit Agent Role Specs and `using-superRA` Surfaces

**Status:** IMPLEMENTED — scope covered role specs (`agents/implementer.md`, `agents/reviewer.md`), the master skill (`skills/using-superRA/SKILL.md`), and every reference under `skills/using-superRA/references/`. Generated direct-mode role references were regenerated from the updated source specs per `CLAUDE.md §Architectural Patterns`.

### Summary of Cuts and Pointers

**`agents/implementer.md`** — 54 lines removed net (~35 % shorter).
- DELETED — §Stage → skills and references wrapper paragraph (already the exhaustive authoritative line in `superRA:using-superra` §Skill-Load Manifest; the wrapper was anti-pattern "wrapper instructions around authoritative content").
- DELETED — §What the dispatch prompt carries — and doesn't "narration" paragraph describing which pieces live where (pure "here is what you will receive" anti-pattern). The behavior-shaping "treat paraphrased dispatch content as over-specification" sentence was KEPT and consolidated into the renamed §Dispatch Inputs section.
- DELETED — §Execution Protocol §Data-First Discipline bullets (describe / log row counts / validate / document decisions) — owned by `econ-data-analysis`. Replaced with a one-line POINTER: "Follow the discipline of the domain skill you loaded for this Stage."
- DELETED — §Execution Protocol §While You Work paragraph (duplicated under §Escalation already).
- DELETED — Step 6 Worktree wrapper in §Before You Start ("If the dispatch includes a `Worktree:` field, follow the canned steering...") — explicitly called out as anti-pattern in Task 5. The substantive behavior is in the dispatch's `Additionally:` line itself and in `agent-orchestration §Parallelization`; the wrapper added nothing.
- DELETED — example nested bullets under Step 1 ("At integration stage, you always load ...; for data analysis work, you load ...") — these are already specified in the Skill-Load Manifest.
- POINTER — §Editing Etiquette three-rule block collapsed to three one-liners and a pointer to `superRA:handoff-doc` for the full discipline. KEPT the inline-edit / task-block-boundary / doc-before-report rules as one-liners because they are behavior-shaping for every implementer edit.
- POINTER — Worktree-return rule in §Handoff trimmed to one line ("Return the `<branch>/parallel/<slug>` branch name and HEAD SHA in your status report. Do not merge, rebase, push, or touch worktree lifecycle — the orchestrator owns harvest-out.") since the lifecycle rules themselves live in `agent-orchestration`.
- KEPT — the "Evidence before claims" IDENTIFY / RUN / READ / VERIFY gate (behavior-shaping and non-default); §What You Own, What You Don't (behavior-shaping ownership rules); §How You Fix Review Items on a REVISE Round (non-default ordering constraint); §Pre-Commit Self-Check checklist (behavior-shaping); §Report Format (field list is behavior-shaping).

**`agents/reviewer.md`** — 38 lines removed net (~20 % shorter).
- DELETED — §Stage → skills and references and §What the dispatch prompt carries wrapper paragraphs (same anti-patterns as in implementer.md).
- DELETED — example nested bullets under the manifest-loading step.
- DELETED — long "The orchestrator populated it at planning time" narration in the §Project Conventions reading step — collapsed to behavior-shaping one-liner ("code that ignores a documented convention is a MAJOR integration-review finding" is KEPT).
- POINTER — §Editing Etiquette three-rule block collapsed to one-liners plus pointer to `superRA:handoff-doc`.
- KEPT — §Severity Levels (authoritative severity rubric), §Verdict protocol (non-default ordering constraint), §How You Write a Review first-review and re-review procedures (non-default), §CRITICAL severity invariant, §Pre-Commit Self-Check, §Report Format.
- Also fixed a stray `into dispatch prompts.` fragment in the frontmatter description (dangling orphan from a prior edit).

**`skills/using-superRA/SKILL.md`** — 1 line tightened.
- Trimmed the over-long paragraph describing where handoff-doc discipline lives (it is now one sentence pointing at the role-spec compact etiquette and `superRA:handoff-doc`).
- KEPT — Runtime Workflow Map, Commit Hygiene, Skill Inventory, Skill-Load Manifest, Instruction Priority. These are all authoritative content owned by this skill.

**`skills/using-superRA/references/main-agent.md`** — §Execution Modes collapsed from 28 lines to 10 lines.
- DELETED — the Codex-agent block at the bottom of §Execution Modes (lines 139-153 pre-edit). It was a near-duplicate of `codex-instructions.md §Delegation Priority in Codex` with a typo (`spwawn`) and a dangling orphan fragment ("when the workflow allows it and the user requested it, the task is trivial, or agent tools are unavailable."). Replaced with a single-line POINTER: "Codex agents: MUST load `references/codex-instructions.md` immediately."
- KEPT — §Session Start Actions, §Load the Handoff-Doc Skill, §Workflow Frontier Resolver (Task 2 owner), §Changes of the Plan (pointer), §Three Pause Classes, §Proceed Without Asking, §Banned Phrasings, §One Question at a Time, §Log Before You Act. All behavior-shaping; most are authoritative for their concern.
- KEPT — Subagent-default sentence and the direct-mode protocol bullets. Behavior-shaping (they shape when the main agent dispatches vs stays inline).

**`skills/using-superRA/references/codex-instructions.md`** — no edits. Audit pass confirmed this file is the authoritative owner of Codex delegation priority, warm-agent lifecycle, and tool-map content; every line is either a non-default constraint or a tool-name mapping. POINTER target check: `main-agent.md`'s §Execution Modes now cites this file correctly.

**`skills/using-superRA/references/claude-tools.md`, `copilot-tools.md`, `gemini-tools.md`** — no edits. These are adapter tool-name mapping tables; every row is behavior-shaping (the tool-name mapping is the content).

**`skills/using-superRA/references/direct-mode-implementer.md`, `direct-mode-reviewer.md`** — regenerated from the updated source specs. Per `CLAUDE.md §Architectural Patterns` (Generated artifacts stay generated), these are not hand-edited. The generator now:
1. No longer reads a `## Stage → skills and references` section (removed from source per Task 6); direct-mode's own §Before You Start step 1 carries the manifest-load instruction.
2. Produces a compact direct-mode §Before You Start that mirrors the trimmed subagent version — POINTER style, no example nested bullets.
3. Drops the cleanup_implementer_execution_protocol and cleanup_implementer_handoff pattern-replace helpers (their targets no longer exist in source); kept the reviewer-side strip_subsection of §Report Format and the "ad-hoc default" deletion because those fragments still appear in the subagent body.

### Verification

- `python3 skills/codex-superra-setup/scripts/sync_codex_agents.py --scope project --check` — PASS.
- `python3 skills/codex-superra-setup/scripts/test_sync_codex_agents.py` — 5 tests pass (generator idempotency, direct-mode round-trip, managed-header presence, conflict handling, regenerate-hint).
- `git diff --check` — clean.
- Cross-check: every POINTER inserted this round points at content that actually exists at the cited source — `superRA:handoff-doc` carries the full editing-etiquette discipline; `econ-data-analysis` (per `econ-data-analysis/SKILL.md §Discipline`) carries Data-First principles; `codex-instructions.md §Delegation Priority in Codex` carries the Codex named-agent rule.

### Files Changed

- `agents/implementer.md`
- `agents/reviewer.md`
- `skills/using-superRA/SKILL.md`
- `skills/using-superRA/references/main-agent.md`
- `skills/using-superRA/references/direct-mode-implementer.md` (regenerated)
- `skills/using-superRA/references/direct-mode-reviewer.md` (regenerated)
- `skills/codex-superra-setup/scripts/sync_codex_agents.py`
- `.codex/agents/superra_implementer.toml` (regenerated)
- `.codex/agents/superra_reviewer.toml` (regenerated)

## Task 7: Audit Workflow Skills and `agent-orchestration`

**Status:** Not started.

## Task 8: Audit Utility, Domain, and Meta Skills

**Status:** Not started.

## Task 9: Cross-Audit Consistency Sweep

**Status:** Not started.
