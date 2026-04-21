# RESULTS — superRA Autoload Hook

## Task 1: Write the autoload-superra hook script

**Status:** IMPLEMENTED

Created `hooks/autoload-superra` (extensionless bash, `chmod +x`) and `tests/hooks/test-autoload-superra.sh`. The hook:

- Parses stdin UserPromptSubmit JSON via python3, extracting `prompt` and `transcript_path` as tab-separated so `IFS=$'\t' read` splits cleanly even when the prompt contains spaces.
- Fast-path `grep -iqE '(^|[^[:alnum:]])super[-_ ]?ra([^[:alnum:]]|$)'` on the prompt. No match → `{}`.
- Transcript gate: `grep -iEq '"skill"[[:space:]]*:[[:space:]]*"superRA:using-superRA"' "$transcript_path"`. Match → `{}`. Fails-open when transcript_path is missing or empty (V5).
- Emits `additionalContext` via the three-way platform branch (`CURSOR_PLUGIN_ROOT` / `CLAUDE_PLUGIN_ROOT` / fallback) matching `merge-guard`, `ask-user-question-logger`, and `exit-plan-mode`.

Regression suite (`tests/hooks/test-autoload-superra.sh`, 12 vectors):

```
PASS  V1 no-mention                                      (got silent)
PASS  V2a superRA                                        (got reminder)
PASS  V2b super RA                                       (got reminder)
PASS  V2c super-ra                                       (got reminder)
PASS  V2d Super_RA                                       (got reminder)
PASS  V2e superra lc                                     (got reminder)
PASS  V2f mid-sentence                                   (got reminder)
PASS  V3 superrapid                                      (got silent)
PASS  V3 superrant                                       (got silent)
PASS  V4 already-loaded                                  (got silent)
PASS  V4b other-superRA-skill                            (got reminder)
PASS  V5 empty-transcript-path                           (got reminder)

Passed: 12    Failed: 0
```

The V4b vector ("transcript shows `superRA:planning-workflow` loaded but not `using-superRA`") deliberately triggers the reminder — the transcript gate is specifically about the master skill.

## Task 2: Register the hook in hooks.json and hooks-cursor.json

**Status:** IMPLEMENTED

Added `UserPromptSubmit` array to `hooks/hooks.json` (Claude Code) routing through `run-hook.cmd autoload-superra`, and a `userPromptSubmit` entry to `hooks/hooks-cursor.json` (Cursor) invoking `./hooks/autoload-superra` directly. Both JSONs validate via `python3 -m json.tool`. No `matcher` field on the Claude Code registration — `UserPromptSubmit` does not scope by tool name.

## Task 3: End-to-end verification in a fresh session

**Status:** Not started
