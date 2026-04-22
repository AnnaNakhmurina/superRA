# Theory-Modeling Vertical — Results

> Mirrors `PLAN.md` structure. Updated after each task with key findings.
> New agents: read `PLAN.md` for what to do, `RESULTS.md` for what was found.

**Last updated:** 2026-04-22 (review fixes incorporated; verification checks passed)
**Status:** In Progress

---

## Task 1: Create the `theory-modeling` domain skill and its stage-scoped references

**Status:** Implemented (review fixes incorporated)

### Key Findings
- Added `skills/theory-modeling/SKILL.md` with a proactive trigger surface, stage-scoped load table, an iron law centered on defined objects plus stated assumptions, and a shared `Define–Derive–Validate` checklist.
- The checklist makes notation discipline, interpretable primitive-level assumptions, stepwise derivations, and proof / special-case / numerical verification `[BLOCKING]` requirements.
- Added planning, drift-test, and integration references so the vertical has a complete PLAN / Phase A / Phase B path without inventing a new workflow or rendering utility.

### Notes
- The skill explicitly points human-facing equation/table/figure rendering back to `superRA:report-in-markdown`.
- Review feedback on the planning template has already been incorporated: the inventory now includes explicit sections for timing / information structure, solution concept, and notation conventions, and the hard gate no longer implies task drafting before researcher approval.

## Task 2: Wire the new vertical into runtime surfaces, docs, and discovery

**Status:** Implemented (review fixes incorporated)

### Key Findings
- Added `theory-modeling` to the `using-superRA` inventory and manifest, the `planning-workflow` routing table, and the `refactor-and-integrate` domain-specific integration pointers.
- Generalized the planning template and exit-plan-mode reminder so a second planning hard gate is first-class rather than a data-only exception.
- Updated contributor/runtime docs (`README.md`, `skills/CATEGORIES.md`, `CLAUDE.md`) to present theory/modeling as an implemented vertical, added `.agents/skills/theory-modeling`, and extended `tests/check-harness-compatibility.sh` with discovery/wiring assertions.

### Notes
- A review pass surfaced remaining single-vertical assumptions in the canonical agent prompts and generic integration/merge references; the task now includes fixes for those surfaces plus stronger compatibility assertions.
- The compatibility suite and targeted smoke checks are being treated as Task 3 verification rather than as Task 2 completion criteria.
- Generated `.codex/agents/*` artifacts were refreshed after the canonical agent prompt updates and are now in sync with the checked-in source prompts.

## Task 3: Verify the new vertical end to end and reconcile any drift

**Status:** Implemented (checks passed; pending final review)

### Key Findings
- `bash tests/check-harness-compatibility.sh` passed on the final wiring state, including the new assertions for theory-modeling coverage in `using-superRA`, `planning-workflow`, the canonical implementer/reviewer prompts, and the generic integration references.
- `python3 skills/codex-superra-setup/scripts/sync_codex_agents.py --scope project --check` passed, confirming the regenerated `.codex/agents/superra_implementer.toml` and `.codex/agents/superra_reviewer.toml` artifacts match source.
- A targeted smoke check passed for repo-local discovery and routing semantics: `.agents/skills/theory-modeling` resolves to the canonical skill directory, `planning-workflow` routes to `superRA:theory-modeling`, `using-superRA` lists the vertical in its manifest, and the `Model Inventory / Assumption Map` hard gate is present in the planning reference.

### Notes
- This verification pass was structural and workflow-facing. It did not run a live Claude/Codex end-to-end dispatch session against a toy modeling prompt in this turn.
