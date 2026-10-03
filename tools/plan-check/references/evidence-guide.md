# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: the plan's "Cause:" statement (first paragraph of the Candidate plan section); the observations it must explain are in the "Repro evidence" section (numbered steps, control runs, debug/log output, "Expected/Actual"). The issue body gives the reported behavior. Live: the plan.md diagnosis section and the quoted repro comment on the thread.
What good looks like: the cause predicts every repro observation, including the controls and any step showing when the damage first happens. A cause that a control run rules out (the behavior still occurs, or doesn't, without the suspect factor), or that only repeats the thread's guess, is not grounded no matter how detailed it is.

## Scope

Where it lives: the plan's "Change:" line and its "In:" / "Out:" statements in the Candidate plan section; any deferral wording. Live: the plan.md scope section.
What good looks like: the listed changes are exactly what fixes the one reproduced behavior, with unrelated work named as out of scope. Red flags: a migration, redesign, new option, new UI, extra framework, "while in the area" cleanup, or many "fronts". A smaller plan that defers the larger rework with a reason is bounded.

## Executability

Where it lives: the plan's named files/functions/components and its approach sentence in the Candidate plan section. Live: plan.md files-to-touch and approach sections.
What good looks like: a stranger can open a named file or function and begin, because one approach is chosen. Red flags: "somewhere", "upstream or vendored, whichever is easier", "investigate", "maybe also check", no files at all.

## Test plan

Where it lives: the "Test:" line of the Candidate plan, read against the numbered steps and Expected/Actual in the Repro evidence. Live: plan.md test plan section.
What good looks like: it re-runs the repro trigger and names an outcome that differs before and after the fix (exit code, text, color, value, failing-then-passing case). Red flags: "run the full suite", "should feel fast", "nothing else should break" with no observable change.

## Honesty

Where it lives: any "Risks / unknowns" wording in the plan, hedges in the cause, and claims in the plan comment; for live runs, the "## Deviations" section at the end of plan.md. Compare against the Repro evidence.
What good looks like: unverified things (other platforms, other call sites, untested variants) are named as unknowns, and certainty is used only where the repro evidence supports it. Red flag: a guess stated as traced fact, or confident language with no unknowns at all.

## Comms

Where it lives: the "Candidate plan comment" section, read against the "Thread highlights" section (maintainer direction, named culprits, requested tests, open PRs) and the "Repo facts" block (contribution policy, AI-use or disclosure rule, bug-report template). Live: comment.md and the live issue thread.
What good looks like: the comment answers or builds on what the maintainer asked, engages (rather than races) any existing PR, makes no date promise, and contains an explicit AI-use disclosure sentence when the repo's policy requires disclosure (or reads as human-voiced and issue-specific when it demands that). Red flags: ignoring an owner's in-thread direction, and no disclosure where the policy requires one.
