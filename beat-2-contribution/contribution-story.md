# Contribution: NVDA Does not Pronounce Completely the last part of the last syllable of the last word or character name After a While Working

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Yaswanth Kottana
**GitHub Username:** Yashu16
**Issue:** https://github.com/nvaccess/nvda/issues/19286
**Pull Request:** https://github.com/nvaccess/nvda/pull/21131

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout, a search-rank-grade funnel over all of GitHub, and patched it after its first scan**

I turned my unit-1 issue-select skill into issue-scout by writing three `gh search issues` queries from my fit profile (AI/ML keywords, plain Python, and Python tooling, all filtered to open, unassigned "good first issue" results) and six ranking filters. Testing the queries by hand first, two of them returned 0 results because I had quoted `"llm OR rag OR ..."` as one phrase; removing the quotes brought each to 100 results. The first scan accepted all five candidates, and that was the problem: my rubric had no check for whether a repo is actually used, so a one-day-old repo with 0 stars passed twice, and my group-by-query ranking let the AI/ML query fill every slot. I added a required "Repo in use" check (fail under 10 stars when the owner opened every recent issue and PR), verified it by hand with gh against four repos, and changed the ranking to take turns across the three queries.

**Unit 6: Installed a voice guard that checks what I post upstream, patched my scout's scope check, and wrote a memory file for the NVDA repo**

I merged my three voice guide copies into one guide, added Upstream rules for claims, questions and pings, and installed the hook; the wiring test blocked a body-less `gh issue comment`, so I knew the hook fired. Two things broke: `python3` on my machine was only the Windows Store stub, so I pointed the hook at `python`, and the guard could not launch the `claude` CLI from Python on Windows because it is an npm `.cmd` shim, so I patched `check_voice.py` to find it with `shutil.which`. Once it ran, it stopped my first comment on the PR because my question asked two things and offered no guess, which broke my own "one question" rule, and I rewrote it as one yes-or-no question before posting. My claim comment on the issue was posted before the guard existed, so the guard never saw it; I wrote a repo memory file for NVDA (`CLAUDE.local.md`, kept out of git with `.git/info/exclude`) with the PR rules, the exact files my issue touches, and the build and test commands, so each session in that repo starts with them loaded; I have not measured how much that saves yet, and the file's orientation section is my own reading of the code rather than an agent run.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My first pick from the Issue Selection site, blixvip/easyedit#36, disappeared on 2026-11-08 when the owner's account and repo were deleted and the project's Discord turned into a crypto scam. I re-picked from the site and checked ten issues by hand with `gh`; eight failed because someone had already claimed them, a PR was already open, or the issue was stale or not a real issue. NVDA #19286 was the one that held up: it is triaged and labelled "good first issue", nobody had claimed it or opened a PR, outsiders get merged (11 of the last 35 merges), and a maintainer, seanbudd, had written the scope in the thread: expose the existing `audio.whiteNoiseVolume` setting in the Audio panel, document it, and document the Realtek fix a user found. I re-graded it with my scout after patching the scope check: it came back reject, but only because my "Not already claimed" check counted my own claim comment as someone else's. Every other required check passed, scope-feasible passed, and the difficulty note was moderate, with the open edge being the 0-100 volume range. Its fit-profile check also failed, since this is a Windows accessibility setting and not AI/ML work. I chose it against that verdict because the reject came from a gap in my rubric, not from the issue, and because the scope is small, bounded and agreed with a maintainer who answers within days. It fits me because it is Python (a wx settings panel, config, tests) and it is a user-facing change someone can use, and I hope to learn how a large project reviews a change from outside: its docs, changelog, translation and lint rules. My scout fell short in three ways on a real search. It rejected my own claim, which I logged as a patch but did not apply. Its AI-policy check looked at two CONTRIBUTING.md paths and passed on silence, so I read NVDA's contributing doc myself. And a second scan for a backup returned nothing usable: the five issues I checked by hand were already claimed, had open PRs, or sat in repos that do not accept outside code, so claim and linked-PR checks belong earlier in the ranking. What convinced me it still helped is that the scope-feasible patch let a real issue with an open edge through, and its difficulty note told me to ask about the open edge.

**Claim comment:** https://github.com/nvaccess/nvda/issues/19286#issuecomment-6717329950 (posted 2026-11-08)

**Toolkit**

- Tool: issue-scout, with the unit 6 scope patch (my `scope-feasible` row and a difficulty note replaced my old "scope is one bounded task" check). I ran it in direct mode on the NVDA issue URL.
- What it produced: reject on NVDA #19286, from one failed required check ("Not already claimed", triggered by my own claim comment). The other required checks passed, the difficulty note was moderate, and the fit-profile check failed.
- Difficulties: the rubric has no exception for my own claim; the AI-policy check missed NVDA's real contributing doc; the second scan's ranking favoured tiny new repos, and I had to run the evidence checks by hand when the nested scout runs kept stalling on permissions.
- What changed: I replaced the scope check as the patch asked and logged the other three patches (ignore my own claims, search more contributing paths, rank by claims and linked PRs) in `scan-notes.md` without applying them yet.

---

## Understanding the Issue

**Problem Description**

With some audio drivers, NVDA cuts off the end of what it says after it has been running a while. The maintainers' explanation in the thread is that NVDA keeps the audio device awake after speech by playing silence, and some drivers trim that silence anyway, which clips the speech. NVDA already has a workaround: it can play very quiet white noise instead (the `audio.whiteNoiseVolume` setting, 0-100), so the driver sees real sound. What is missing is a way for a user to set it. The issue stayed open at the maintainer's request, to make that setting public and to document the driver fix a user found.

**Expected Behavior**

The Audio category of NVDA settings has a "White noise volume" control next to "Time to keep audio device awake after speech", the User Guide explains it (including the Realtek workaround from the thread), and the changes file lists it.

**Current Behavior**

The setting exists in the configuration and the audio code, but the Audio panel has no control for it, and I found no mention of it in the User Guide or the changes file. In the thread the maintainer gave a user a Python console command to set it.

**Affected Components**

- `source/config/configSpec.py:68`: the setting, `whiteNoiseVolume = integer(default=0, min=0, max=100)`.
- `source/nvwave.py:~367`: plays white noise at that volume when the awake time is above 0.
- `source/gui/settingsDialogs.py`: `AudioPanel`, where the awake-time control is built (`makeSettings`) and saved (`onSave`).
- `source/_bridge/runtimes/synthDriverHost/synthDriverHost.py:77`: already syncs the key.
- `user_docs/en/userGuide.md` and `user_docs/en/changes.md`.

**Toolkit**

- Tool: Claude Code with the `gh` CLI, which I used to read all of the issue thread and to find where the setting lives in the code.
- What it produced: a plain-language summary of a long thread (about 25 comments over six months) and the list of files above.
- Difficulties: the issue's title is about clipped speech, but the agreed work is only a missing control. I first described my PR as addressing the issue, until I re-read the thread: for the reporter the white noise only delayed the problem, and what fixed it was turning off Windows audio enhancements.
- What changed: my PR says it exposes and documents the setting, and does not claim to fix clipped speech.

---

## Reproduction Process

**Environment Setup**

- OS: Windows 11 Home Single Language 10.0.26200, 64-bit laptop with a Realtek audio driver (6.0.9427.1).
- NVDA: run from source, upstream `master` at commit 7b58df9 (before my change), cloned with submodules.
- Python 3.13.17 (64-bit), uv 0.13.7, Visual Studio Build Tools 2026 (18.4) with the C++ workload, Clang, ATL and ARM64 components.
- Setup: `uv sync`, then `scons source`, then `runnvda.bat`.
- Challenges: my first `uv sync` failed because NVDA pins `python-preference = "only-system"` and I only had Python 3.12 and 3.14, so I installed Python 3.13.17 and it worked. `scons source` first failed from Git Bash because `cmd` could not find `scons.bat` by a relative path; running it by its full path fixed that. My Build Tools install also lacked the Clang, ATL and ARM64 components in NVDA's `.vsconfig`, which I added with the Visual Studio Installer.

What I am reproducing: the issue asks, after the maintainer's comments, for the existing `audio.whiteNoiseVolume` setting to be exposed in the Audio settings panel. What I can reproduce is that gap: the setting exists in NVDA but cannot be changed in the settings window. I did not try to reproduce the clipped speech itself, and I make no claim about whether the white noise fixes it.

**Steps to Reproduce**

1. Build and start NVDA from source on commit 7b58df9 (`scons source`, then `runnvda.bat`).
2. Open NVDA menu > Preferences > Settings, and select the Audio category.
3. Look at the controls after "Time to keep audio device awake after speech (seconds)". Observed: that box (set to 30) is the last control, and there is no control for white noise volume.
4. Check the code on the same commit: `git grep -n whiteNoiseVolume 7b58df9 -- source`. Observed: the setting is defined and used, and nothing under `source/gui` mentions it.

**Reproduction Evidence**

- **Commit showing reproduction:** none. The reproduction ran on unmodified upstream master, commit 7b58df9 (https://github.com/nvaccess/nvda/commit/7b58df9fb). The lines below are at that commit.
- **Screenshots/logs:** screenshot of NVDA Settings: Audio on commit 7b58df9, where the last control is "Time to keep audio device awake after speech (seconds)" with value 30 and nothing after it. Output of the grep:
  ```
  source/config/configSpec.py:68:  whiteNoiseVolume = integer(default=0, min=0, max=100)
  source/nvwave.py:367:            c_float(config.conf["audio"]["whiteNoiseVolume"] / 100.0),
  source/_bridge/runtimes/synthDriverHost/synthDriverHost.py:77:  ("audio", [..., "whiteNoiseVolume"])
  ```
- **My findings:** the setting exists in the configuration and in the audio code, but I found no control for it in the Audio panel. The maintainer's comments in the thread describe the same gap, and say it can be set from the NVDA Python console. I did not reproduce the clipped-speech symptom from the issue.

**Toolkit**

- Tool: repro-check, with the wild scope file from unit 6. I ran it on `claim-draft.md` and `repro-draft.md` together.
- What it produced: accept, with all six required checks passing, and notes on weak spots.
- Difficulties: it flagged that "users can only change it from the Python console" claimed more than my evidence showed, since I had not checked whether editing `nvda.ini` also works. It also pointed out that my "after my change" sentence belonged in the PR, not the repro. Its voice-guide notes said my posted claim had no date to report back by and named nothing out of scope, because I posted it before the voice guard existed.
- What changed: I softened the claim to "I found no control for it in the Audio panel" and removed the after-change sentence from the repro. For the next comment I post on the issue, I will check it with the voice guard first.

---

## Solution Approach

**Analysis**

The clipped speech comes from the user's audio driver trimming silence, which NVDA cannot fix, as the maintainer says in the thread. The part NVDA can fix is that its own workaround is hidden: `whiteNoiseVolume` is defined in `configSpec.py` and used in `nvwave.py` (`wasSilence_playFor(..., whiteNoiseVolume / 100.0)`), but `AudioPanel` in `settingsDialogs.py` never creates a control for it, so there is nothing for a user to reach.

**Proposed Solution**

Add a spin control to `AudioPanel` right after the awake-time control, built the same way (`SelectOnFocusSpinCtrl`, min and max read from the config spec, a help binding), and save its value in `onSave`. Document it in the User Guide with the Realtek audio enhancements tip, and add a changes entry. I am deliberately not changing how the white noise is played, the default (0, off), or anything in `nvwave.py`, and I am not claiming it fixes the clipped speech.

**Implementation Plan**

1. `source/gui/settingsDialogs.py`: add `whiteNoiseVolumeEdit` after `audioAwakeTimeEdit` and save it in `onSave`.
2. `user_docs/en/userGuide.md`: add a "White noise volume" section with the anchor `{#WhiteNoiseVolume}` so F1 on the control opens it.
3. `user_docs/en/changes.md`: one entry under "Changes".
4. `tests/unit/test_audioSettingsPanel.py`: three tests for the control.

Deviations: my first label was "White noise volume (0 to disable):", and I changed it to "(set to 0 to disable)" because it could be read as a range. The lint step wrapped two long lines, and NVDA's autofix bot then added a trailing comma to each, which I copied into my commit. I rebased onto the newest upstream master before opening the PR.

**Toolkit**

- Tool: Claude Code, which wrote the control, the docs, the changes entry and the tests; I read the diff, ran NVDA from source, and committed and pushed it myself.
- What it produced: a 17-line change in `AudioPanel`, a 17-line User Guide section, a changes line and a 63-line test file.
- Difficulties: F1 on the control said "no user guide found" until I built the docs with `scons user_docs`, because running from source has no generated `userGuide.html`; it was not a bug in my anchor.
- What changed: I rebuilt the docs, retested F1, and confirmed the generated HTML contains `WhiteNoiseVolume`.

---

## Testing Strategy

Before and after, in the same environment: on upstream commit 7b58df9, Settings > Audio ends with "Time to keep audio device awake after speech (seconds)" and nothing after it. On my branch, the same panel shows "White noise volume (set to 0 to disable):" after it, with value 0. I also checked by hand that a value I set and saved was still there when I reopened the panel, that Alt+W focuses the control, that the label and value are spoken when I tab to it, and that F1 opens the new User Guide section.

Automated checks, from NVDA's own commands: `rununittests` ran 1529 tests and passed (5 skipped), including three new tests in `tests/unit/test_audioSettingsPanel.py` (the control shows the saved value, its range matches the config spec, and saving writes the value to the config); all three fail without my change. `runlint` (ruff and pyright), `runcheckpot` (my label has its translator comment) and `runlicensecheck` all pass. The same checks, 48 of 48, passed on a practice PR in my fork. NVDA's CI on the real PR is waiting for NV Access to approve the first run.

Not tested: whether the white noise stops clipped speech on affected hardware, braille display output, or end-to-end system tests.

**Toolkit**

- Tool: NVDA's own test, lint, pot and license scripts, run through Claude Code, plus my fork's GitHub Actions.
- What it produced: the results listed above, and a failing-then-passing check on my new tests.
- Difficulties: `runcheckpot.bat` failed from Git Bash because it calls `scons` by its short name; I ran its two steps by hand, `scons pot` then `tests/checkPot.py`. My first force push was refused with "stale info" because the autofix bot had added a commit to my branch that I did not have.
- What changed: I applied the bot's two trailing commas to my commit and pushed with an explicit lease on the commit I had inspected.

---

## Implementation Notes

**2026-11-08**

My first pick (easyedit#36) disappeared, so I re-picked, chose NVDA #19286 and posted my claim. seanbudd answered the same day that the scope was fine and pointed me to NVDA's AI policy: PRs must be human-submitted and tested by the author.

**2026-11-09**

Forked and cloned NVDA, read its contributing doc, and found the Audio panel code. Wrote the control, the User Guide section and the changes entry. Wrong turn: I first told myself the awake-time block (lines 3762-3776) only held a label; it builds the whole control, so the new one goes right after it. Installed uv, Python 3.13.17 and the missing Visual Studio components, ran `scons source`, and took the before and after screenshots of the Audio panel.

**2026-11-10**

F1 said "no user guide found" until I built the docs. Added unit tests, ran lint, unit tests, pot and license checks, rebased onto the newest upstream master, and opened a practice PR in my fork, where all 48 checks passed. Caught two mistakes in my own PR text before opening the real one: a bare `#19286` in the fork PR would have linked to the real issue, and a "CONFIRM" note meant for me was left inside the description. Opened the draft PR (#21131) and posted a comment on the issue with the link and one question about the 2027.1 feature freeze.

**Toolkit**

- Tool: Claude Code for the code and tests, `gh` for reading the issue and PR state, and my voice guard for the comment on the issue.
- What it produced: the PR and the checked comment.
- Difficulties: environment setup took most of the time (uv would not download Python, `scons.bat` was not found from Git Bash, missing Visual Studio components), and the voice guard first stopped my comment because my question asked two things at once.
- What changed: I rewrote the question as one yes-or-no question, and I patched the guard so it can run `claude` on Windows.

---

## Pull Request

**PR Link:** https://github.com/nvaccess/nvda/pull/21131 (draft, opened 2026-11-10)

**PR Description:** "Add White noise volume setting to the Audio settings panel" follows NVDA's template: it links `Closes #19286`, summarizes the missing control, lists the user-facing change (the new control, the User Guide section with the Realtek tip, the changes entry), says there are no developer-facing changes, describes the approach and testing, states that I used an AI assistant and reviewed and tested the change myself, and lists known issues (no controlled test on affected hardware, the 2027.1 feature-freeze question, no live preview of the volume).

**Maintainer Feedback:**

- 2026-11-08: seanbudd confirmed the scope ("That would be great, thank you") and pointed me to the AI policy and contributing guidelines.
- 2026-11-10: I opened the draft PR and commented on the issue with the link and a question about whether the changes entry belongs under "Changes" for 2027.1. No reply yet, and NVDA's CI is waiting for NV Access to approve the first run.

**Status:** Awaiting review (draft).

**Toolkit**

- Tool: Claude Code drafted the description from NVDA's template; I edited it, and I opened the PR myself. I did not run my `pr-precheck` on this PR: its scope file still names the course's Path Review repo and refuses other repos, and the unit 6 materials gave me wild scope files only for repro-check and plan-check, so I checked the description by hand against NVDA's template guide instead.
- What it produced: the description above and the comment on the issue, which my voice guard passed.
- Difficulties: the template has an instruction for AI agents to add an "unsupervised AI PR" header, which is false for my PR, so I left it out and wrote an honest AI-assistance line instead. I also caught the stray link and the "CONFIRM" note before creating the PR.
- What changed: I check a PR description for stray links and notes to myself before creating it.

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

- NVDA's contributing guide, which set the PR rules, the human-submitted and author-tested requirement, and the size limit: https://github.com/nvaccess/nvda/blob/master/projectDocs/dev/contributing.md
- NVDA's guide to creating the dev environment, which listed Python 3.13.17, uv and the Visual Studio components: https://github.com/nvaccess/nvda/blob/master/projectDocs/dev/createDevEnvironment.md
- NVDA's PR template guide, for the description sections: https://github.com/nvaccess/nvda/blob/master/projectDocs/dev/githubPullRequestTemplateExplanationAndExamples.md
- The issue thread, where seanbudd explained the driver trimming silence and agreed the scope, and shadyar found the Realtek audio enhancements fix: https://github.com/nvaccess/nvda/issues/19286
- The existing "Time to keep audio device awake" control in `AudioPanel`, which was the pattern for my control, and `tests/unit/test_voiceSettingsPanel.py`, which was the pattern for my tests.
