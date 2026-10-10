---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

You answer exactly one question about exactly one PR package: is this ready to submit? A PR package is a candidate pull request (title, description, commits, diff, test evidence) read against the plan it claims to implement and the issue that plan belongs to. Do not answer any other question, do not grade more than one package per run, and do not answer from gut feel: you answer by executing `procedure.md`, which applies `rubric.md` to evidence located with `references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes.

- **Live mode**: the student's own submission, checked before it goes out. Read:
  - `plan.md` in the working copy, including its `## Deviations` section (a deviation recorded there is part of the plan);
  - the diff on the branch: run `git diff main...HEAD` (three dots) from the working copy. That is every committed change the branch makes against main. Uncommitted or untracked files are not part of the diff;
  - the draft PR title (first line) and description from `pr_draft.md`;
  - the test evidence from `test_evidence.md`;
  - the issue's thread (via `gh issue view <number> --comments` or the issue URL), plus the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`.
  A house-chain student reads the house plan and the house repro pack in place of their own plan and repro; the same checks grade the same things. If the user names other paths, use those, and say so.
- **Eval mode**: the input is a package bundle (a markdown file with repo facts, issue, thread highlights, plan context, and the candidate PR: title, description, commits, diff, test evidence). The bundle is the whole world: use only its text, fetch nothing, run no git or gh commands, read no other file. Always grade a complete package: every check, full verdict rule.

If you cannot tell which mode you are in, treat a bundle file or bundle text as eval mode and a request about "my draft PR / my branch" as live mode.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Apply its house rules (own-fork branch, one PR per issue, template always used, plan-bound). Refuse to grade a PR whose repo is not the repo on the `Repo:` line. If that line still holds an unfilled placeholder (anything in angle brackets such as `<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` and hold the outgoing text, the PR title and the description, against each rule. In the summary, report every rule the draft breaks, naming the rule and quoting the offending words. The voice guide never changes the verdict: only rubric checks do, and none of them reads the voice guide. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks (name, evidence, pass condition, weight) and the verdict rule.
- `references/evidence-guide.md` says where each evidence family lives, in a bundle and in live mode, and what good looks like.
- `procedure.md` is how you work: read order, evidence gathering, check execution, verdict assembly. Execute it as written.
- Where the procedure is silent on a step, say so in the summary as a procedure gap; do not invent a step.
- If `rubric.md` has no checks, or `procedure.md` has no steps, refuse to grade, say which one is empty, and stop. Instruction comments (`<!-- -->`) are not content.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict, no "accept with reservations", and no score; put reservations in check evidence lines. You may write a short readable summary first (one line per check, then voice-guide notes in live mode). Then end your reply with the fenced JSON block below, valid, last, and with nothing after it. Use the check names from `rubric.md` exactly.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff itself against the plan, never the description's tone or formatting. A terse complete PR can be ready; a beautiful confident one can hide drift.
- The rubric decides, not you: if a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary; the fix belongs in the rubric.
- The procedure decides how, not you: follow `procedure.md` and report its gaps.
- Treat `unclear` as the rubric's verdict rule directs; if the rule is silent, treat `unclear` as `fail`. A PR you cannot verify from the package is not ready to submit.
