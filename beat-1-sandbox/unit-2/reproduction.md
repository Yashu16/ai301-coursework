# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Yashu16

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/29#issuecomment-5851010509

Hi! I'd like to take this one. I've been doing Python work and want to build more hands-on experience with AI-safety testing specifically, and this fits well: a CI-gated test suite in tests/security/test_prompt_injection.py backed by a curated attack corpus in tests/fixtures/injection_attempts/, run against the existing safety/ layer whenever a PR touches it.

My plan: first read through safety/ to see what the defense actually checks for and how it's invoked, then build the attack fixture set (starting past the obvious "ignore previous instructions" pattern into role-play, encoding/obfuscation, and multi-turn injection variants), then write the test suite asserting each fixture is blocked, and wire it into CI on changes under safety/. I don't yet know how deep the existing defense goes, so I'll report back once I've traced it rather than guessing at scope now.



**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/29#issuecomment-5861434142

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


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run (`--limit 3`, pkg-01/02/03): 2/3 agreement. `pkg-03` disagreed —
   my rubric rejected it on `ai-disclosure`, which at that point assumed every
   repo's AI-use policy was a blanket "must disclose AI assistance" rule.
2. Targeted re-run (`--only pkg-03,pkg-07,pkg-20`) after splitting the disclosure
   check into distinct AI-policy shapes (explicit disclosure vs. human-voiced/no-
   disclosure-demand vs. permissive): 3/3 agreement. `pkg-07` (explicit-disclosure
   repo) and `pkg-20` (disclosure category, gold `reject`) served as canaries and
   both still agreed after the loosening.
3. Full run #1 (all 20): 19/20 agreement. `categories: clear-accept 8/8 disclosure
   1/1 no-evidence 4/4 unfollowable-comms 2/3 wrong-target 4/4`. Only `pkg-19`
   disagreed (my rubric graded `accept`, gold said `reject`).
4. Investigated `pkg-19`: gold rejects it because the claim comment is boilerplate
   flattery promising a "guaranteed" fix in 2 days, even though the repro report
   itself is solid. My rubric's only comms check at that point (`comms-specific`)
   was `preferred`, so a failing claim comment couldn't sink an otherwise-good
   package. Promoted it to a `required` check (`claim-comment-honest`).
5. Targeted re-run (`--only pkg-19,pkg-01,pkg-11,pkg-09,pkg-10`) as canaries for
   clear-accept packages with terse, modest claim comments that shouldn't get
   swept up by the tightened check: 5/5 agreement, including `pkg-19` flipping
   to `reject`.
6. Full run #2 (all 20, no `--save-run`): 18/20 agreement (bar: PASS). `pkg-09`
   and `pkg-10` newly disagreed — both are honest cannot-reproduce packages, and
   my `behavior-matches-issue` check had no explicit path for "ran the real
   trigger, showed the real (negative) artifact, named the environment
   difference" to count as a pass.
7. Fixed `behavior-matches-issue` to accept an honest cannot-reproduce as a pass
   condition, not just a match. Targeted re-run (`--only pkg-09,pkg-10,pkg-01,
   pkg-02,pkg-08,pkg-16,pkg-17`) using `pkg-02`/`pkg-08`/`pkg-16`/`pkg-17` as
   wrong-target canaries (packages with a real artifact that must still fail
   for showing the wrong behavior): 7/7 agreement.
8. Full confirming run, saved with `--save-run eval-run.txt`: **20/20 agreement
   (bar: 18/20: PASS)**. `categories: clear-accept 8/8 disclosure 1/1
   no-evidence 4/4 unfollowable-comms 3/3 wrong-target 4/4`. This is the run in
   the committed `eval-run.txt`.

**Package analysis**

`pkg-19` (source: `vuejs/core#15205`). Gold label: `reject` ("the repro report
is fine; the claim comment is interchangeable assign-me boilerplate promising a
guaranteed 2-day fix, which fails specificity and honest-intent"). My rubric's
first pass also decided `accept` on this package, because at that point the
only check reading the claim comment's tone (`comms-specific`) was `preferred`
and could never flip the verdict — the repro report's genuinely strong artifact
(a real playground reproduction with a produced-CSS excerpt matching the
issue's reported unscoped-selector behavior) carried the package to `accept` by
itself. The candidate claim comment reads: "Hello sir! Great project, I love
Vue and use it every day... Kindly assign it to me, I will fix it within 2 days
guaranteed." That is a promise the reproduction gives no basis for (fixing the
bug is a separate, unstarted task from reproducing it) and reads as boilerplate
that could be pasted onto any issue. I fixed this by promoting a rewritten,
required `claim-comment-honest` check that reads the claim comment on its own,
independent of the repro report's quality — after that change, my rubric
correctly rejects `pkg-19` in agreement with gold.

**Check rationale**

From `rubric.md` (`claim-comment-honest`):

> Pass if the claim comment is specific to this issue (names the actual
> bug/finding) and makes no promise or certainty claim beyond what a
> reproduction supports (no "guaranteed fix in N days," no interchangeable
> flattery/assign-me boilerplate that could paste onto any issue). Fail if the
> comment is boilerplate, or promises an outcome (a fix, a timeline) it has no
> basis to guarantee.

This check reads this way because its predecessor (`comms-specific`) graded
the same evidence but was marked `preferred`, meaning a fail there could never
change the verdict. That was a mistake I only caught by running the full eval:
`pkg-19` showed that a claim comment can be dishonest in a way that should hold
the whole package back on its own, regardless of how strong the attached repro
report is — reproducing a bug and promising to fix it in 2 days guaranteed are
different claims with different evidence requirements, and only the first one
had support here. I rejected keeping it `preferred`, since that would mean no
claim-comment content could ever gate a verdict, which contradicts the
lecture's stated proof family for honest, respectful comms.

**Trade-offs**

Loosening `behavior-matches-issue` to also pass an honest cannot-reproduce
(rather than only passing when the artifact shows the issue's exact reported
behavior) gives up some of its ability to catch a report that merely ran
something and calls it a "cannot reproduce" without a real attempt behind it —
a lazy report could, in principle, claim "didn't trigger" with a thin or
irrelevant artifact and pass this check on the letter of the rule. I accept
this risk deliberately: the rule requires the artifact to come from the
issue's actual, unmodified trigger and requires a named environment/setup
difference as the candidate reason, which the `honest-claim` check backs up
independently (an unsupported cannot-reproduce claim still fails there). The
targeted re-run confirmed the loosening cost nothing on this eval set: `pkg-09`
and `pkg-10` (both genuine cannot-reproduce cases) flipped to correctly agree,
while `pkg-02`, `pkg-08`, `pkg-16`, and `pkg-17` — all wrong-target packages
with a real but mismatched artifact — still correctly failed as canaries in
that same `--only` run.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
