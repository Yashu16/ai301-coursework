# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment line, read against the issue's stated target (version, OS, platform specifics the issue names as relevant). | Pass if the report states the specific versions/OS/platform actually used to reproduce. Fail if there is no environment record, or if a version deviation from the issue's target is present but never acknowledged. | required |
| steps-followable | The repro report's steps, read as a stranger who has never seen the codebase would read them: starting state, exact commands/inputs, in order. | Pass if a stranger could re-run the exact trigger from the steps alone (no missing setup, no "run the project" hand-waving, no private/unshared resources). Fail if a required setup step, config, or resource is missing, private, or unshared. | required |
| behavior-matches-issue | The artifact (output excerpt, console log, screenshot) in the repro report, read against the issue's stated actual-behavior description. | Pass if EITHER: the artifact shows the same behavior the issue describes (same error, same trigger condition), produced by running the issue's actual, unmodified trigger; OR the report is an honest cannot-reproduce, i.e. it ran the issue's real trigger faithfully, shows the artifact of that real attempt (even though it doesn't show the bug), and names what differs from the issue's environment/setup as a candidate reason. Fail if the steps were altered (different syntax, different flag, different input) such that the artifact shows a different failure mode than the one reported, with no acknowledgment of the change. | required |
| honest-claim | The claim comment and repro report's stated conclusion, read against what the artifacts actually show. | Pass if the report claims exactly what its evidence supports — including an honest "cannot reproduce" backed by a real attempt and a named difference from the issue's environment. Fail if the comment asserts a stronger conclusion (root cause, "guaranteed reproducible," certainty) than the shown artifacts back up. | required |
| ai-policy-conditions | The repo-facts block's stated AI-use/contribution policy for comments, read against the claim comment and repro report's text. | Pass if the repo states no AI-related condition on comments, or if it does and the comment satisfies it: a disclosure requirement is met by an explicit disclosure sentence, and a "written by a human, in your own words / understand what you post" requirement is met by a comment that reads as human-voiced and specific (not a policy statement copy-pasted, not a generic disclosure substituting for genuine understanding). Fail if the stated condition is unmet. | required |
| claim-comment-honest | The claim comment's own content, read on its own (independent of whether the repro report is strong). | Pass if the claim comment is specific to this issue (names the actual bug/finding) and makes no promise or certainty claim beyond what a reproduction supports (no "guaranteed fix in N days," no interchangeable flattery/assign-me boilerplate that could paste onto any issue). Fail if the comment is boilerplate, or promises an outcome (a fix, a timeline) it has no basis to guarantee. | required |

## Verdict rule

Accept if every `required` check passes. Any `required` check that fails
or is `unclear` causes reject. `preferred` checks (comms-specific) never
change the verdict; report them for the summary only. `unclear` is
treated as fail for every required check: evidence that cannot be found
is not evidence that the package is ready.
