# Procedure: how this skill grades a PR package

## Read order

1. Live mode only: read `scope.md` first and apply the scope seam. Eval mode skips this step.
2. Read the repo's stated asks: the repo-facts block in eval mode; live, `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`. Note every template section, issue-link ask, changelog or whatsnew ask, listed check, and AI-use disclosure rule.
3. Read the issue and thread highlights. Note the one behavior reported and every maintainer direction.
4. Read the plan before the diff and before the description. Note: the files named, the in-scope list, the out-of-scope list, every promised change, the test plan's trigger and expected-after, and (live) the `## Deviations` section. This list is what the diff is judged against, so write it down before you open the diff.
5. Read the diff. Live: run `git diff main...HEAD`. List every changed file and every hunk with a one-phrase purpose.
6. Read the commit list, then the test evidence, then the description and title last, so claims are judged against facts already collected.
7. Live mode only, at the end: read `voice-guide.md` for the voice seam.

## Evidence gathering

1. diff-matches-plan: put the plan's file and promise list beside the diff's hunk list. Mark each hunk "in plan", "in deviation note", or "extra". Mark each plan promise "present" or "missing" and, if missing, whether a note or the description discloses it.
2. description-matches-diff: list each claim in the title and description ("exactly as planned", "no other changes", "adds tests", "updates docs"). Beside each, write true or false from the diff.
3. test-evidence-is-decisive: copy the plan's test trigger and expected-after. Find in the evidence the command or steps that were run, whether it is that trigger or a control or another path, and the before and after outputs. Note any failure mode the plan's test plan names that the evidence skips.
4. repo-checks-run: list the checks the template or docs name. For each, record the outcome shown in the evidence, or "not shown". Record every ticked box and whether evidence backs it.
5. diff-is-reviewable: scan every hunk for debug prints, commented-out code, dead or unused functions, TODOs, formatting or import churn, and unrelated hunks. Record each with file and line.
6. template-and-policy-met: for each template section and each policy ask from Read order step 2, record the description text that fills it, or "blank/boilerplate". Record the disclosure sentence, or "none". Record whether a required changelog or similar file appears in the diff.
7. maintainer-direction-engaged: record each maintainer direction and whether the PR engages it.
8. title-is-specific: record whether the title names the change.
9. In eval mode use only the bundle text. Never fetch anything.

## Check execution

1. Run the checks in the order of the rubric table.
2. Grade each check from the facts recorded in Evidence gathering; re-read the package only when a fact is missing.
3. Apply the pass condition literally. If any fail condition is met, grade fail even if the PR is polished, confident, or has a perfect checklist. If the pass condition is met, grade pass even if the PR looks thin.
4. Compare, do not trust: a description saying "exactly as planned" or "no other changes" counts for nothing until the diff has been compared with the plan.
5. A disclosed deferral or deviation that the diff matches passes; the same gap undisclosed fails.
6. If the evidence a check needs is genuinely absent from the package, grade unclear and name what is missing.
7. Write one evidence line per check: the quote or fact that decided it.

## Verdict assembly

1. Collect the grades of the required checks.
2. If every required check is pass, the verdict is accept. If any required check is fail or unclear, the verdict is reject (unclear counts as fail).
3. Preferred checks are reported but never change the verdict.
4. If several required checks fail, quote the first failing one in rubric order as the deciding check, and list the others with their evidence lines.
5. Live mode: after the check summary, add the voice-guide notes, then emit the JSON block last.
