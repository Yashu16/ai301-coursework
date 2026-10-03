# Procedure: how this skill grades a plan package

## Read order

1. Read the repo-facts block first. Note: the contribution policy, any AI-use or disclosure rule, and any template asks.
2. Read the issue body and thread highlights. Note: the one behavior the issue reports, and every maintainer direction (a requested action, a named culprit, an existing PR or prior art, "please test X").
3. Read the repro-evidence block before the plan. List every observation as a short numbered line, and mark the controls (runs without the suspect factor), the isolating steps, and any debug/log/timing output. This list is what the cause is judged against, so it must exist before you read the cause.
4. Read the candidate plan: cause, change, in/out scope, files, test.
5. Read the candidate plan comment last, against steps 1-2 and the plan.

## Evidence gathering

1. For cause-fits-evidence: copy the plan's cause as one sentence. Then, for each numbered repro observation, write whether the cause predicts it (yes / no / not addressed). Pay most attention to controls.
2. For change-is-bounded: list every distinct change the plan commits to doing. Write the single behavior the repro isolates. Mark each change as "needed for that behavior" or "extra". Note any explicit deferral and whether it gives a reason.
3. For stranger-can-start: record the named files/functions and the one chosen approach. Record every phrase that postpones a decision ("somewhere", "whichever", "maybe", "investigate first", "TBD").
4. For test-is-decisive: record the test's exact trigger and the exact observable outcome it names. Check whether that outcome differs before vs. after the fix, and whether it comes from the repro evidence's steps.
5. For honest-uncertainty: record the plan's stated unknowns and any claim worded as certain. Compare each certain claim with the repro list from Read order step 3.
6. For comment-fits-thread-and-repo: record (a) each maintainer direction from step 2 and whether the comment engages it, (b) whether the repo-facts block requires AI disclosure or human voice, and if so the exact sentence in the comment that satisfies it (or "none").
7. For comment-matches-plan: record any difference in cause, scope, or test between the comment and the plan, and any promised date.
8. Use only the package text. Never fetch anything in eval mode.

## Check execution

1. Run the checks in the order of the rubric table.
2. Grade each check only from the facts recorded in Evidence gathering; do not re-read the whole package unless a fact is missing.
3. Apply the pass condition literally. If the pass condition is met, grade pass even if the plan feels weak; if its fail condition is met, grade fail even if the plan is polished, long, or confident.
4. If the needed evidence is genuinely absent from the package (for example, no repro evidence to compare the cause with), grade unclear and name what is missing.
5. A bounded plan that explicitly defers bigger work with a reason passes change-is-bounded. A plan that bundles extra work fails it, even if its core fix is right.
6. Write one line of evidence per check: the quote or fact that decided it.

## Verdict assembly

1. Collect the grades of the required checks.
2. If every required check is pass, the verdict is accept.
3. If any required check is fail or unclear, the verdict is reject (unclear counts as fail).
4. The preferred check is reported but never changes the verdict.
5. In the output, quote the deciding evidence line for each failed or unclear check, then emit the JSON block last.
