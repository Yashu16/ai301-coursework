# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/29

**Verdict output**

```
## Summary

All three candidates are from the scoped repo (`codepath/pathreview-ai301-fa26-s3`), all opened by the same collaborator (`Aburke225`), all currently unclaimed (no assignees, no linked/mentioned PRs, no comments at all), and all pass every required check. The repo itself is clearly alive — pushed 3 days ago, no archive flag, no AI-contribution ban anywhere in `CONTRIBUTING.md`.

**Ranked (all accepted):**
1. **#30 – Bias audit report** (`scripts/audit_bias.py`) — best fit: this is a data/eval-pipeline task (sample stored data, run through detector, produce a report), closest to the "data/eval pipelines" part of the stated fit profile.
2. **#29 – Red-teaming test suite for prompt injection** — good fit, but leans more toward test/CI authorship than core model logic.
3. **#25 – Cross-request bypass detection for safety monitoring** — safety-flavored but closer to monitoring/pattern-matching plumbing than ML/eval work.

All three also carry the preferred "good-first-issue signal" (opened by a maintainer/collaborator, even without a literal label).

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/30",
    "checks": [
      {"name": "Repo not dead", "grade": "pass", "evidence": "pushed_at 2026-09-16, archived: false"},
      {"name": "Maintainer responsive", "grade": "pass", "evidence": "5 default-branch commits within last 30 days, last push 3 days before capture"},
      {"name": "Scope is one bounded task", "grade": "pass", "evidence": "single deliverable: implement scripts/audit_bias.py producing a bias report; no TBD/umbrella language"},
      {"name": "Not already claimed", "grade": "pass", "evidence": "no assignees, 0 comments, GitHub search for PRs referencing #30 returned total_count: 0"},
      {"name": "AI contribution policy allows this workflow", "grade": "pass", "evidence": "docs/CONTRIBUTING.md sets conditions (CI green, tests, docstrings) with no AI-ban statement"},
      {"name": "Good-first-issue signal", "grade": "pass", "evidence": "opened by Aburke225, author_association: COLLABORATOR"},
      {"name": "Matches my fit profile", "grade": "pass", "evidence": "task is a data/eval pipeline script sampling reviews and reporting detector accuracy"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/29",
    "checks": [
      {"name": "Repo not dead", "grade": "pass", "evidence": "pushed_at 2026-09-16, archived: false"},
      {"name": "Maintainer responsive", "grade": "pass", "evidence": "5 default-branch commits within last 30 days, last push 3 days before capture"},
      {"name": "Scope is one bounded task", "grade": "pass", "evidence": "single deliverable: automated red-teaming test suite in tests/security/, no TBD/umbrella language"},
      {"name": "Not already claimed", "grade": "pass", "evidence": "no assignees, 0 comments, GitHub search for PRs referencing #29 returned total_count: 0"},
      {"name": "AI contribution policy allows this workflow", "grade": "pass", "evidence": "docs/CONTRIBUTING.md sets conditions, no AI-ban statement"},
      {"name": "Good-first-issue signal", "grade": "pass", "evidence": "opened by Aburke225, author_association: COLLABORATOR"},
      {"name": "Matches my fit profile", "grade": "pass", "evidence": "Python security/test-writing task adjacent to safety/ML work, though more test-authorship than model logic"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/25",
    "checks": [
      {"name": "Repo not dead", "grade": "pass", "evidence": "pushed_at 2026-09-16, archived: false"},
      {"name": "Maintainer responsive", "grade": "pass", "evidence": "5 default-branch commits within last 30 days, last push 3 days before capture"},
      {"name": "Scope is one bounded task", "grade": "pass", "evidence": "single deliverable: add cross-request pattern detection in safety/monitoring.py, no TBD/umbrella language"},
      {"name": "Not already claimed", "grade": "pass", "evidence": "no assignees, 0 comments, GitHub search for PRs referencing #25 returned total_count: 0"},
      {"name": "AI contribution policy allows this workflow", "grade": "pass", "evidence": "docs/CONTRIBUTING.md sets conditions, no AI-ban statement"},
      {"name": "Good-first-issue signal", "grade": "pass", "evidence": "opened by Aburke225, author_association: COLLABORATOR"},
      {"name": "Matches my fit profile", "grade": "pass", "evidence": "Python safety-monitoring task, but closer to monitoring/pattern-matching plumbing than core ML/data work"}
    ],
    "verdict": "accept"
  }
]
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke run (`--limit 3`): 2/3 agreement — issue-01 failed on "Maintainer responsive" and "Scope is one bounded task."
2. Full run #1 (all 20): 13/20 agreement — below the 18/20 bar. `categories: claimed 4/4 clear-accept 2/8 dead-repo 3/3 policy 1/1 scope 3/4`. The "Maintainer responsive" check (originally: pass only if ≥2 of 5 sampled issues got an owner/member/collaborator reply within 30 days) was too strict and was failing several gold-accept issues on healthy, actively-committed repos whose 5-issue response sample happened to be sparse.
3. Targeted re-run (`--only issue-01,issue-06,issue-09,issue-14,issue-16,issue-19,issue-20`) after loosening "Maintainer responsive" to also pass on recent push/commit activity, and tightening "Scope is one bounded task" to catch undetermined-design requests: 5/7 agreement — issue-06, issue-09, issue-14, issue-16 flipped correct; issue-20 flipped correct; issue-01 and issue-19 still disagreed.
4. Full run #2 (all 20): 19/20 agreement. `categories: claimed 4/4 clear-accept 7/8 dead-repo 3/3 policy 1/1 scope 4/4`. Only issue-01 still disagreed.
5. Full confirming run, saved with `--save-run eval-run.txt`: **18/20 agreement (bar: 18/20: PASS)**. `categories: claimed 4/4 clear-accept 6/8 dead-repo 3/3 policy 1/1 scope 4/4`. issue-01 and issue-04 disagreed this time — the grader (Sonnet) has some run-to-run variance on borderline calls, but the rubric held above the bar with the category floor met in every category. This is the run in the committed `eval-run.txt`.

**Issue analysis**

`issue-01` (source: conda/conda#16475). Gold label: `accept` ("docs task with a stated home and scope; active repo, unclaimed"). My rubric's decision on the final saved run: `reject`, failing "Scope is one bounded task."

The issue asks for a new permanent docs page plus updates to three existing docs files (`manage-pkgs.rst`, `pip-interoperability.rst`, `new-features.md`), and separately suggests "considering" a global troubleshooting entry as a lower-priority nice-to-have. My rubric's scope check reads the multiple concrete file-level changes plus the softly-worded optional addition as scope creep resembling an umbrella/tracking issue, even though every sub-item serves one stated outcome (giving the GA `conda install`-from-PyPI workflow a permanent docs home) and the maintainer/opener treats it as one piece of work rather than a list to be split into separate tracked issues. The gold label reads the same evidence as one bounded documentation task because all sub-items are already fully specified by the opener, nothing is left for a future decision, and a docs restructuring across a few related pages is normal scope for a single first PR. My check's threshold for "how many named sub-changes is still one task" is stricter than the gold standard's, so it fails an issue whose scope is bounded but wide.

**Check rationale**

From `rubric.md` (Maintainer responsive):

> Fail only if the sample shows zero owner/member/collaborator comments within 90 days across all 5 sampled issues AND the last push to any branch is more than 30 days before the capture date. A repo with recent commit/push activity (within 30 days) passes even if the response sample itself is sparse, since active maintainers who are merging code are not the same signal as a dead tracker with an unanswered issue queue. Pass otherwise. `unclear` if the sample has fewer than 5 entries with usable dates and there is no recent push to fall back on.

This check exists in this relaxed form because the first version (pass only if ≥2 of 5 sampled issues got a reply within 30 days) was rejecting gold-accept issues on repos that were obviously alive — e.g. `conda/conda`, which had 5 fresh default-branch commits within days of capture but whose 5-issue response sample happened to show mostly "no maintainer comment in thread." A busy maintainer merging code daily is a stronger liveness signal than a small random sample of unanswered issues, so the check now treats recent commit/push activity as an alternate, sufficient path to "pass," and only fails on the conjunction of a stale response sample *and* a stale push date.

**Trade-offs**

Loosening this check to accept on recent push activity alone gives up its ability to catch a repo that ships code but ignores its issue tracker entirely — a maintainer merging their own PRs and dependency bumps (e.g. dependabot activity) while never responding to community issues would now pass "Maintainer responsive" on push recency, even though a first-time contributor's PR might sit unreviewed indefinitely. I accept this miss deliberately: none of the 20 eval issues exercised that specific pattern (a repo with committed code but a fully unresponsive human issue-review process), so the relaxation cost nothing on this eval set — confirmed by the targeted `--only` re-run, where every gold-accept issue this check had wrongly failed (issue-06, issue-09, issue-14, issue-16) flipped to correct, with no new false accepts introduced among the reject-gold issues in that same re-run.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. I've mainly worked in Python and want to grow toward core AI/ML and safety-adjacent work, and I specifically wanted something with more depth than a trivial one-line bug fix this time. Issue #29 (build an automated red-teaming test suite that runs known prompt-injection attacks against the safety layer in CI) fits that: it's Python, it's AI-safety-flavored, and its stated effort estimate (7-10 hours) is a real but manageable scope for the time I have.
2. My skill's verdict correctly identified that the repo is alive (pushed days before I checked, no archive flag), that the issue is unclaimed (no assignee, no comments, no linked PRs), that it's one bounded deliverable (a test suite in a named file path, not an open-ended design question), and that the repo's contribution policy sets conditions rather than banning AI-assisted work. What the rubric couldn't weigh is that #29 is more test/QA authorship than core model-logic work, compared to #30's data/eval-pipeline framing — that's a fit judgment about what kind of work will be a better resume story for me, which is outside what an accept/reject rubric is built to decide, and I chose it anyway because building a red-teaming suite is a distinct, currently in-demand skill (adversarial testing of LLM systems) that I don't have direct experience with yet.
3. The main anticipated difficulty is curating a good attack corpus: I'll need to research realistic prompt-injection techniques (not just the obvious "ignore previous instructions" pattern) and make sure the test suite is asserting the right thing against the actual `safety/` implementation, rather than writing tests that pass trivially. Since the issue has zero comments and no prior attempt, I'll be defining the shape of "curated set of known attacks" myself with no existing discussion to lean on.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
