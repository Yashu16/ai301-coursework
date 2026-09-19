# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo not dead | Repo facts: `last push to any branch`, `latest release`, and `last 5 default-branch commits`, dated against the bundle's `captured` date (or today, in live mode) | Fail if `archived: yes`. Otherwise fail if the last push to any branch is more than 180 days before the capture date AND none of the last 5 default-branch commits fall within that same 180-day window. Pass otherwise. | required |
| Maintainer responsive | Repo facts: `maintainer first-response sample` (5 recently updated issues), plus `last push to any branch` and `last 5 default-branch commits` | Fail only if the sample shows zero owner/member/collaborator comments within 90 days across all 5 sampled issues AND the last push to any branch is more than 30 days before the capture date. A repo with recent commit/push activity (within 30 days) passes even if the response sample itself is sparse, since active maintainers who are merging code are not the same signal as a dead tracker with an unanswered issue queue. Pass otherwise. `unclear` if the sample has fewer than 5 entries with usable dates and there is no recent push to fall back on. | required |
| Scope is one bounded task | Issue body and comment thread | Fail if the issue is explicitly a tracking/umbrella issue (a checklist of sub-items meant to be split up), if the thread shows the design/spec is still being actively debated with no maintainer decision recorded, if a maintainer states the fix touches core internals, or if the request itself names an undetermined design/asset ("TBD", "possibly ... if needed", scope still being decided by the reporter rather than settled). Fail if the issue is a pure usage/support question with no requested code or doc change. A bug report that names multiple related root causes or improvement ideas for the *same* underlying defect (e.g. "two potential causes, here's how to fix each") is still one bounded task; only fail multi-item issues when the items are independent deliverables, not sub-causes of one problem. Pass otherwise, regardless of how terse or polished the write-up is. | required |
| Not already claimed | Repo facts: `this issue: assignees` and `linked PRs`; issue comment thread | Fail if an assignee is set, or if any linked PR is open, or if the thread contains a claim comment ("I'll take this" / "working on this" / "can I work on this") dated within the 60 days before capture that a maintainer did not contradict. A closed/unmerged linked PR alone, or a claim comment older than 60 days with no recent follow-up, does not fail this check. `unclear` only if the thread is truncated such that recent comments cannot be read. | required |
| AI contribution policy allows this workflow | Repo facts: `contribution policy` line | Fail only on an explicit statement that AI-generated code/documentation/contributions are not accepted. Pass if the policy states conditions (disclosure, human review, testing) instead of a ban, if it only carries an `AGENTS.md`-style file (instructions for agents, not a ban), or if there is no stated policy at all (silence passes). | required |
| Good-first-issue signal | Issue labels; who opened the issue | Pass if the issue carries a `good first issue` (or equivalent) label, or was opened/filed by a maintainer (owner/member/collaborator). | preferred |
| Matches my fit profile | Issue body/title and repo `description` | Pass if the work is Python-flavored and/or AI/ML/RAG/data-pipeline flavored per `scope.md`'s fit profile. | preferred |

## Verdict rule

Accept only if every `required` check grades `pass`. Any `required` check
graded `fail` rejects the issue. `unclear` on a required check is treated as
`fail` (the default from SKILL.md): a first issue whose liveness, scope,
claim status, or policy status cannot actually be verified from the
evidence is not one to take blind. `preferred` checks never affect the
verdict; they only order accepted issues against the fit profile, with
`Good-first-issue signal` weighted first and `Matches my fit profile`
second when ranking ties.
