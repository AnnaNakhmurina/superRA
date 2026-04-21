# RESULTS — superRA Autoload Hook

## Task 1: Write the autoload-superra hook script

**Status:** IMPLEMENTED

Created `hooks/autoload-superra` (extensionless bash, `chmod +x`) and `tests/hooks/test-autoload-superra.sh`. The hook:

- Parses stdin UserPromptSubmit JSON via python3, extracting `prompt` and `transcript_path` as tab-separated so `IFS=$'\t' read` splits cleanly even when the prompt contains spaces.
- Fast-path `grep -iqE '(^|[^[:alnum:]])super[-_ ]?ra([^[:alnum:]]|$)'` on the prompt. No match → `{}`.
- Transcript gate: `grep -iEq '"skill"[[:space:]]*:[[:space:]]*"superRA:using-superRA"' "$transcript_path"` — case-insensitive, whitespace-tolerant around the colon so minor format drift does not produce duplicate reminders. Match → `{}`. Fails-open when transcript_path is missing or empty (V5).
- JSON-escapes the reminder text through `python3 -c 'import json,sys; print(json.dumps(sys.stdin.read()))'` before splicing into the payload, so inner `"` / `\` in the reminder cannot invalidate the JSON. The three-way platform branch (`CURSOR_PLUGIN_ROOT` / `CLAUDE_PLUGIN_ROOT` / fallback) matches `merge-guard`, `ask-user-question-logger`, and `exit-plan-mode` while emitting the already-quoted JSON string.

Regression suite (`tests/hooks/test-autoload-superra.sh`, 16 vectors — V6 family added per review finding 3; every non-empty payload asserted via `python3 -m json.tool` per finding 2):

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
PASS  V6a embedded-dquote                                (got reminder)
PASS  V6b embedded-bslash                                (got reminder)
PASS  V6c multiline-prompt                               (got reminder)
PASS  V6d non-ascii                                      (got reminder)

Passed: 16    Failed: 0
```

The V4b vector ("transcript shows `superRA:planning-workflow` loaded but not `using-superRA`") deliberately triggers the reminder — the transcript gate is specifically about the master skill. V6a–d cover JSON-special and non-ASCII prompts; the hook does not embed the prompt into its output, but the vectors fence against a future regression that would.

## Task 2: Register the hook in hooks.json and hooks-cursor.json

**Status:** IMPLEMENTED

Added `UserPromptSubmit` array to `hooks/hooks.json` (Claude Code) routing through `run-hook.cmd autoload-superra`, and a `userPromptSubmit` entry to `hooks/hooks-cursor.json` (Cursor) invoking `./hooks/autoload-superra` directly. Both JSONs validate via `python3 -m json.tool`. No `matcher` field on the Claude Code registration — `UserPromptSubmit` does not scope by tool name.

## Task 3: Write the two PreToolUse workflow-skill gate hooks + tests

**Status:** Not started

## Task 4: Register the two new hooks in hooks.json and hooks-cursor.json

**Status:** Not started

## Task 5: End-to-end verification in a fresh session

**Status:** Not started
