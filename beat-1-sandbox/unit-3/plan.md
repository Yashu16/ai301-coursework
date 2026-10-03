# Plan: red-teaming test suite for the prompt injection defense (issue #29)

## Diagnosis

There is no bug here; the gap is missing coverage. Issue #29 asks for an automated suite that throws known prompt injection attacks at the safety layer and checks they are blocked, run in CI. Today `safety/prompt_defense.py` (`PromptDefense.is_injection_attempt()`) is only exercised by its own unit test, `tests/unit/test_prompt_defense.py`; there is no `tests/security/` and no CI job for it.

Repro evidence I rely on (from my unit 2 repro comment on #29):

- "confirmed it's only exercised by its own existing unit test today — nothing in `agent/`/`api/` calls it yet, so this suite tests the layer directly, matching the issue's wording."
- The defense checks: separator lines, `System:`/`Human:`/`Assistant:` role-switching, `{{ }}`/`{% %}` template injection, ignore/forget/disregard/override phrasing, and `execute()`/`run()`/`eval()` calls.
- Prototype run: `pytest tests/security -v --tb=short` gave `23 passed, 1 xfailed in 0.38s`; `LLM_PROVIDER=mock pytest tests/unit -q` gave `375 passed, 53 xfailed`.
- One real gap found while building the corpus: `"System  :  ignore"` (whitespace before the colon) is not caught. That is already tracked as D-04, a separate issue, so it stays an `xfail`, not part of this change.

So the cause of the issue's problem (no regression net on the safety layer) is simply that nothing runs attacks against it; the defense itself blocks everything in my corpus except the D-04 case.

## Scope

In scope:
- `tests/fixtures/injection_attempts/`: five JSON corpus files (role switching, instruction override, template/code injection, benign inputs that must not be flagged, known gaps).
- `tests/security/test_prompt_injection.py`: parametrized test over the corpus; attacks must be blocked, benign inputs must pass, known gap is a strict `xfail`.
- `.github/workflows/ci.yml`: one new `security-red-team` job running `pytest tests/security`.

Not in scope:
- Fixing the D-04 whitespace-before-colon bypass or any other change to `safety/prompt_defense.py`.
- Wiring `PromptDefense` into `agent/` or `api/` (nothing calls it today).
- Changing other CI jobs, test config, or adding dependencies.

## Files I will touch

- `tests/fixtures/injection_attempts/*.json` (new)
- `tests/security/__init__.py`, `tests/security/test_prompt_injection.py` (new)
- `.github/workflows/ci.yml` (add one job)

## Approach

1. Keep the corpus as data (JSON, one category per file) so it can grow without editing test code.
2. One parametrized test class loads every file; attack cases assert `is_injection_attempt(...) is True`, benign cases assert `False`, the known-gap case is `xfail(strict=True)` citing D-04 so it fails loudly when D-04 is fixed.
3. Add the CI job next to the existing ones. The issue says "every PR that touches `safety/`"; the existing workflow's triggers are not path-scoped per job and the suite runs in under a second, so I will run it on every PR rather than add path filtering. If a maintainer wants path filtering I will add it.
4. Run lint/format/typecheck from the Makefile on the new files before committing.

## Test plan

Re-run of my unit 2 repro steps against the real change:

1. Before: on `main` without the change, `pytest tests/security` collects nothing (directory does not exist), and `ci.yml` has no security job.
2. After: `pytest tests/security -v --tb=short` must report `23 passed, 1 xfailed`.
3. Regression: `LLM_PROVIDER=mock pytest tests/unit -q` must still report `375 passed, 53 xfailed`.
4. Proof the suite catches regressions: temporarily weaken one pattern in `safety/prompt_defense.py` (for example delete the `ignore|forget|disregard|override` pattern); `pytest tests/security` must now FAIL on the instruction-override cases. Revert the edit afterwards.
5. `make lint`, `make format`, `make typecheck` clean on the new files.

## Risks and unknowns

- I have not checked whether the CI job passes on GitHub's runner (I only ran it locally on Windows with Python 3.14; CI uses 3.11).
- The corpus (24 cases) covers the patterns the current regexes target; I have not claimed it covers attacks the regexes were never meant to catch (encoding/obfuscation, multi-turn); those would show up as new gaps.
- Unknown whether maintainers want path-filtered CI instead of running on every PR.

## Deviations

Small, nothing that changes the posted plan:

- The suite was already prototyped in my unit 2 repro, so the build was committing that work on branch `test/29-prompt-injection-red-team` and re-running the test plan against it, not writing it fresh.
- `tests/security/__init__.py` already existed on `main`, so I did not add it (the plan listed it as new).
- I ran the mutation check from the test plan: changing `(?:Ignore|Forget|Disregard|Override)` in `safety/prompt_defense.py` made 4 instruction-override cases fail (`4 failed, 19 passed, 1 xfailed`); reverted, back to `23 passed, 1 xfailed`.
- Still unverified: the CI job on GitHub's runner (Python 3.11).
