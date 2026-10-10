# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives: the plan's scope, in-scope/out-of-scope statements, files, and promised changes. Eval: the "Plan context" section. Live: `plan.md`, including `## Deviations`. Compare against the diff: eval, the "Diff" section; live, `git diff main...HEAD`. The description's fidelity claims ("exactly as planned", "no changes beyond the plan", "docs updated") live in the description (eval "Description"; live `pr_draft.md`) and are read against the diff too.
What good looks like: every changed file and hunk falls inside the plan's scope or a deviation note, and every promised change is in the diff or its absence is disclosed. Silent drift runs both ways: more than the plan (a new flag, a rename pass, a rewrite of a file the plan never names) or less (a promised docs update missing) while the description claims fidelity. A description claiming more or less than the diff delivers is drift, not a comms problem.

## Test evidence (harness category: not-tested)

Where it lives: eval, the "Test evidence" section of the candidate PR; live, `test_evidence.md`. Read against the plan's test plan (eval "Plan context"; live the test plan in `plan.md`) and the repro's trigger and expected-after. The repo's own checks are named in the repo-facts block (eval) or `.github/PULL_REQUEST_TEMPLATE.md` (live).
What good looks like: the plan's own repro trigger is re-run on the branch, with an observable before and after (output, exit code, failing-then-passing test) and each listed repo check's outcome visible. Red flags: "tested locally", "tests pass" with nothing named, evidence that exercises a control or the unchanged path, a failure mode the plan named but the evidence skips, a ticked Testing box with no output behind it. A failing check reported honestly with its reason is evidence.

## Diff quality (harness category: unreviewable)

Where it lives: eval, "Diff" and "Commits"; live, `git diff main...HEAD` and `git log main..HEAD --oneline`.
What good looks like: only the change and its tests are in the diff. Debris tells: debug prints (print, eprintln, console.log), commented-out code, dead or unused functions (including behind allow(dead_code)), stray TODOs, import reordering, re-indent or re-printed identical lines, drive-by edits to unrelated files. Commit messages like "wip" or "misc cleanups" support the finding but do not decide it.

## Standards and comms (harness category: standards-wall)

Where it lives: the repo's asks are in eval "Repo facts" (PR-template asks, contribution policy, AI-use policy) and live in `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`; the issue thread is eval "Thread highlights" and live the issue page. The PR honors them in the description (eval "Description"; live `pr_draft.md`) and sometimes in the diff (a changelog or whatsnew entry).
What good looks like: each template section has real content or an honest N/A with a reason, the issue is linked (`Closes #N` or the repo's form), required file entries are in the diff, only true boxes are ticked, and an AI-use disclosure in the author's own words is present (Notes for Reviewers in live mode). Red flags: blank or boilerplate sections, a missing required entry, no disclosure where a policy asks for it, explicit maintainer direction ignored.
