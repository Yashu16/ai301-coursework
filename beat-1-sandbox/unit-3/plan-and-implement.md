# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Yashu16

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/29#issuecomment-5973678635

Here is my plan for this one, built from my earlier reproduction on this thread.

**What I found:** the safety layer (`PromptDefense.is_injection_attempt()` in `safety/prompt_defense.py`) is only exercised by its own unit test, and nothing in `agent/` or `api/` calls it yet. So there is no test suite or CI job that throws attacks at it. In my prototype run, every attack in my corpus was blocked except one known case (whitespace before the colon, `"System  :  ignore"`), which is already tracked as D-04.

**Plan:**
- Add a curated corpus under `tests/fixtures/injection_attempts/` (JSON, one category per file: role switching, instruction override, template/code injection, benign inputs that must not be flagged, known gaps).
- Add `tests/security/test_prompt_injection.py`: parametrized over the corpus; attacks must be blocked, benign inputs must pass. The D-04 case stays a strict `xfail` so it fails loudly when D-04 is fixed.
- Add one `security-red-team` job to `.github/workflows/ci.yml`. I plan to run it on every PR rather than path-filter to `safety/`, because the suite runs in under a second; tell me if you'd rather have the path filter.

**Not doing:** fixing D-04 or any change to `safety/prompt_defense.py`, and wiring the defense into `agent/` or `api/`.

**How I'll check it:** `pytest tests/security -v` should give 23 passed, 1 xfailed (that is what my prototype gave), `pytest tests/unit` should be unchanged (375 passed, 53 xfailed), and I'll temporarily weaken one regex in `safety/` to confirm the suite fails.

**What I haven't verified:** that the CI job passes on GitHub's runner (I only ran locally on Python 3.14, CI uses 3.11), and whether the corpus misses attack styles the current regexes weren't built for, such as encoding or multi-turn attacks. No promised date.

Drafted with help from Claude Code; I read and ran everything described above.

---

## Your branch

**Branch**

test/29-prompt-injection-red-team

**Evidence**

Repro steps from unit 2 (build the suite, run it, check no regression), re-run against the built change.

Before (main, suite stashed):

```
$ ls tests/security
__init__.py
$ python -m pytest tests/security -q
no tests ran in 0.34s
$ grep -c security-red-team .github/workflows/ci.yml
0
```

After (branch `test/29-prompt-injection-red-team`, commit 55f4f6f):

```
$ python -m pytest tests/security -v --tb=short
...
======================== 23 passed, 1 xfailed in 0.69s ========================
$ LLM_PROVIDER=mock python -m pytest tests/unit -q
375 passed, 53 xfailed, 1 warning in 9.52s
```

Mutation check (changed `(?:Ignore|Forget|Disregard|Override)` in safety/prompt_defense.py to `(?:ZZIgnore|ZZForget|ZZDisregard|ZZOverride)`, then reverted):

```
$ python -m pytest tests/security -q --tb=no
FAILED tests/security/test_prompt_injection.py::TestPromptInjectionRedTeam::test_attack_is_blocked[override-03]
FAILED tests/security/test_prompt_injection.py::TestPromptInjectionRedTeam::test_attack_is_blocked[override-04]
4 failed, 19 passed, 1 xfailed in 0.32s
(after revert) 23 passed, 1 xfailed in 0.39s
```

`ruff check tests/security`: All checks passed. `black --check tests/security`: 2 files would be left unchanged.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

19/20. One full run was needed. Before it I made one single-package smoke run (`--only pkg-02`, agreed, not saved) to confirm the harness worked on Windows. The 19/20 run is the one in `eval-run.txt`: "agreement: 19/20 scored items  (bar: 18/20: PASS)".

**Package analysis**

pkg-14 (zellij reattach leak). Gold label: accept ("honestly scoped-down: reattach handshake fix with a regression-window repro; defers the untestable Windows variant and says so"). My rubric rejected it, failing `stranger-can-start`. The plan's Files paragraph says "exact functions to be pinned in the PR after tracing the query issuance with debug logs", and my pass condition fails a plan whose location or approach is deferred to build time ("Fail if the location is "somewhere", the approach is "investigate first"..."). My rubric read that sentence as a postponed decision. The gold label treats it as acceptable because the plan does name the area (the client attach path in `zellij-server` and `zellij-client`) and commits to one approach (drain OSC responses before wiring input), with only the function names left to find. This is the one disagreement.

**Check rationale**

`| stranger-can-start | The plan's named files/areas, chosen approach, and order of work. | Pass if the plan names where the change goes (specific files, functions, or components) AND commits to one approach, so a stranger could open the code and start without asking the author. Fail if the location is "somewhere", the approach is "investigate first" or "whichever is easier", or real decisions are deferred to build time. | required |`

It reads that way because the unbuildable packages (pkg-10, pkg-17, pkg-18) are plans where every real decision is deferred to build time, so I tied the check to two things a stranger needs: a named location AND one chosen approach. I rejected a looser wording like "the plan is specific enough", because that is a judgment about the write-up's feel and graders disagree on it. pkg-14 is the cost of the strict wording: it also catches a plan that names the area and approach but not yet the exact function.

**Trade-offs**

`stranger-can-start` costs me pkg-14, as above. I accept that it will miss, and I did not loosen the check to buy it back, because loosening "location is deferred" would risk flipping the unbuildable packages (pkg-10, pkg-17, pkg-18), which all agreed. I did not run a loosened version with `--only`, so I cannot claim it would have been safe; the bar was already met at 19/20 and every category had a match.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
