# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/122

**Branch**

`test/29-prompt-injection-red-team`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `agreement: 19/19 scored items` (1 package, pkg-05, errored: not a full run, nothing saved)
2. `agreement: 19/19 scored items` (same pkg-05 error, not saved)
3. `agreement: 18/19 scored items` (after pointing the harness at claude.exe; 1 package errored, not saved)
4. Crashed with a UnicodeEncodeError on pkg-05 (no score)
5. `agreement: 19/20 scored items  (bar: 18/20: PASS)` (first clean full run, saved to eval-run.txt)

The errors were my Windows setup, not the rubric: the harness could not launch `claude`, then encoded pkg-05's stdin as cp1252. I fixed both in a wrapper script outside the repo, and did not edit the harness or the skill between runs 3 and 5.

**Package analysis**

pkg-08 (lazygit#5883, stash prompt on untracked-only files). Gold label: `accept`. My rubric said `reject`, with the table note `failed: repo-checks-run`.

Why it read that way: the repo-facts block says lazygit's template requires a requirements checklist, including "code formatted". The description lists "code formatted" as met, but the test evidence only shows the integration test, `go test ./...` and `go generate ./...`. No formatter output appears. My `repo-checks-run` check fails when a listed check's outcome is never shown, so it failed this one. The gold label accepts it because the fix matches the plan's smallest-surface option and the evidence is otherwise decisive. This was the only disagreement in the saved run.

**Check rationale**

From `tools/pr-precheck/rubric.md`:

> | repo-checks-run | The test evidence read against the checks the repo's PR template or contributing docs list (tests, lint, typecheck, and similar). | Pass if each listed check's actual outcome is visible in the evidence, and a failing or unavailable check is reported honestly with its reason. Fail if a listed check is never shown, or a Testing box is ticked with no evidence behind it. | required |

Why it reads that way: I split "was it tested" into two checks. `test-evidence-is-decisive` asks whether the plan's own repro was re-run with a before and after. This one asks whether the repo's listed checks were actually run, so "tests pass" with nothing named and a ticked box with no output behind it both fail here. I wrote the pass condition to accept a failing or unavailable check that is reported honestly with its reason, so it does not turn "less than everything" into a hold. I rejected a looser wording ("checks mentioned") because the not-tested packages claim success without showing output.

**Trade-offs**

`repo-checks-run` costs me pkg-08: it flips a gold-accept package to reject because "code formatted" is claimed in the description but no formatter output is shown. I accept that miss. Loosening the check to forgive claimed-but-unshown checks would also let through the not-tested packages (pkg-04, pkg-10) that say "tested locally" with no output, and the score (19/20, bar 18) is already above the bar, so I made no further changes after the saved run.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
