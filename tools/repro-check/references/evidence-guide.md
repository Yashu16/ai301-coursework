# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives: the "Environment:" line at the top of the repro report
(eval bundle: the candidate repro report section). Live mode: the
student's draft repro comment.

What good looks like: a specific version/build of the tool or library,
OS/platform, and any other dimension the issue names as relevant
(browser, driver, language setting), stated as what was actually run —
not copied from the issue. If the version used differs from the version
the issue was filed or confirmed against, the report says so explicitly
("filed against 1.9.4; still present on 1.11.7"). A missing environment
line, or a deviation never mentioned, fails this check even if
everything else about the report is strong.

## Steps

Where it lives: the "Steps:" section of the repro report.

What good looks like: numbered, concrete actions (exact commands, exact
code, exact config) that start from a state anyone could reach —
public repo, published package, a snippet pasted in full — and end at
the trigger the issue describes. No step may point at a private
resource (an unshared monorepo, an internal config file) or say "set up
the project" without commands. A control run (the same steps with the
one variable that matters removed or changed) is a strong positive
signal but its absence alone does not fail this check; an unfollowable
step does.

## Behavior shown

Where it lives: the console/output/log excerpt inside the repro
report's steps, read side by side with the issue's "actual behavior" or
error text.

What good looks like: the artifact contains the same error type, same
message shape, or same observable symptom the issue names — produced by
running the issue's actual trigger, not a modified one. Watch for a
changed input, flag, or syntax (a swapped operator, a different range
syntax, an unbound variable) that produces a superficially similar but
different failure (a graceful validation error instead of a crash, a
compile error instead of the reported runtime behavior). If the steps
were altered from the issue's own reproduction recipe, the artifact is
evidence about the modified case, not the reported one, even if the
report narrates it as confirmation.

An honest cannot-reproduce is a pass here too, not a fail by default:
if the report ran the issue's real, unmodified trigger, shows the
artifact of that real attempt (even though the bug did not appear), and
names a candidate reason (an environment or setup difference from the
issue), that counts as the behavior check being satisfied faithfully —
there is no bug artifact to show because none occurred, and that is the
finding. Only fail this check when the trigger itself was altered
without acknowledgment, or when there is no real attempt shown at all.

## Honesty

Where it lives: the claim comment's stated intent/conclusion and the
repro report's "Expected"/"Actual" or closing sentence, read against
what the artifact in the same report actually contains.

What good looks like: the words claim exactly what the artifact shows —
no more. An honest "I could not reproduce this" is a pass when it is
backed by a real, followable attempt and names what differed from the
issue's environment (a hypothesis for why it didn't trigger counts in
its favor but isn't required). A fail looks like: a root-cause diagnosis
asserted with no artifact behind it, "guaranteed reproducible" with no
repro shown, or a report that shows one thing (e.g., the tool merely
starting) while claiming to show another (e.g., the reported hang).

## Comms

Where it lives: the claim comment text, read against the issue's own
description and the repo-facts block's stated bug-report template and
contribution/AI-use policy.

What good looks like, AI-policy conditions: read the repo-facts block's
policy text carefully — it names one of a few distinct shapes, and the
check must match the shape actually stated, not assume disclosure is
always what's required:
- **Explicit disclosure requirement** (e.g. p5.js, ghostty): the repo
  requires stating that AI assistance was used. Pass only if the
  comment or report contains an explicit disclosure sentence. Treat
  every eval package's candidate comment as AI-assisted work for this
  check, per the harness's framing.
- **Human-voiced / human-in-the-loop requirement, no disclosure demand**
  (e.g. ripgrep: "comments must be written by humans in their own
  words"; AI-generated comments may be hidden): there is no sentence to
  disclose anything — the bar is that the comment reads as genuinely
  human-authored and specific to this issue (references the owner's
  actual pointers, states real next steps), not that it names AI use.
  Do not fail a comment here for lacking a disclosure sentence; that
  would be enforcing the wrong repo's rule.
- **Permissive / no stated condition** (e.g. conda): passes
  automatically regardless of AI involvement.
Get this from the repo-facts block's contribution-policy line each
time; do not default to assuming disclosure is always the requirement.

What good looks like, claim-comment honesty: the claim comment names the
actual bug or finding in its own words and states a concrete next step,
rather than reading as boilerplate that could be pasted onto any issue
("same here", "+1", generic flattery, "assigning myself, will fix soon"
with nothing concrete backing it). This is required and independent of
the repro report's quality: an excellent repro report does not rescue a
claim comment that promises a guaranteed fix or timeline it has no
basis for, or that is otherwise interchangeable boilerplate — the claim
comment is itself part of what gets posted, and a stranger reading only
it should come away with a specific, credible sense of what this
contributor found and intends to do next.
