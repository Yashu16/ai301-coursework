Environment: Python 3.14.3, pytest 9.1.1, ruff 0.16.9, black 26.5.1, mypy 2.3.1, Windows 11 (via Git Bash), repo installed with `pip install -e ".[dev]"`.

What I did: read `safety/prompt_defense.py` to see what `PromptDefense.is_injection_attempt()` actually checks (separator lines, `System:`/`Human:`/`Assistant:` role-switching, `{{ }}`/`{% %}` template injection, ignore/forget/disregard/override phrasing, and `execute()`/`run()`/`eval()` calls), then confirmed it's only exercised by its own existing unit test today — nothing in `agent/`/`api/` calls it yet, so this suite tests the layer directly, matching the issue's wording.

Built the red-teaming suite:

- `tests/fixtures/injection_attempts/`: 5 JSON files, 24 cases — `role_switching.json`, `instruction_override.json`, and `template_and_code_injection.json` (18 attacks total, including a combined multi-pattern attack and case-varied attempts), `benign_inputs.json` (5 legitimate inputs that must not be flagged), and `known_gaps.json` (1 case).
- `tests/security/test_prompt_injection.py`: loads the fixtures, parametrizes over them, asserts every attack is blocked and every benign input passes through.

While building the corpus I found the existing test file (`tests/unit/test_prompt_defense.py`) already has an `xfail` for a whitespace-before-colon bypass (`"System  :  ignore"` isn't caught), tracked separately in the manifest as its own bug (not this issue). I kept that case in my corpus as `known_gaps.json`, marked `xfail` with the same reasoning, rather than silently dropping it or having it fail my suite for a bug that belongs to a different issue.

Ran the suite:

```
$ pytest tests/security -v --tb=short
...
23 passed, 1 xfailed in 0.38s
```

Also confirmed no regression in the existing suite:

```
$ LLM_PROVIDER=mock pytest tests/unit -q
...
375 passed, 53 xfailed, 2 warnings in 10.69s
```

Ran the repo's required local checks (`make lint`, `make format`, `make typecheck`) against my new files: `ruff check` passed clean, `black` reformatted `test_prompt_injection.py` once and is now clean, `mypy safety/ --ignore-missing-imports` reports no issues.

Wired CI: the `test-unit` job only ran `pytest tests/unit`, so I added a `security-red-team` job to `.github/workflows/ci.yml` running `pytest tests/security -v --tb=short`, since the issue asks for the suite to run in CI. It runs on every PR alongside the other jobs rather than being path-filtered to `safety/` changes only, since this workflow's `pull_request`/`push` triggers aren't path-scoped per job today and the suite runs in well under a second, so there's no cost to running it unconditionally.

Expected: a curated set of known prompt injection attacks against the safety layer, all blocked, running in CI.

Actual: all 18 attack cases plus the combined multi-pattern attack are detected and blocked by the current `PromptDefense.is_injection_attempt()`; all 5 benign cases correctly pass through unflagged; the one known pre-existing gap (whitespace before the role-switch colon) is captured as an `xfail` rather than a silent miss, so it will surface loudly (`XPASS(strict)`) if it's ever fixed under its own issue.
