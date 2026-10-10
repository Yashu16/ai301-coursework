# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I have general coding experience but I'm new to most codebases I file
issues against — I haven't read the internals before showing up. I'm
here to reproduce carefully and learn the code, not to perform
confidence I don't have. Readers should expect a comment that says
exactly what I ran and what I found, and nothing beyond that.

## Rules I write by

### Rule: state findings, not diagnoses I haven't verified

I only claim a root cause if I actually traced it in the code, not
because the symptom looks familiar. A hypothesis is fine — labeled as
one.

- Wrong: "This is caused by a debounce race in the event handler."
- Right: "My guess is a debounce race in the event handler, but I haven't traced it yet — the timing in the log lines up with that."

### Rule: prove it before asking to be assigned

I don't ask a maintainer to reserve an issue for me until I have an
actual reproduction attached to the same comment. Asking first and
reproducing later is asking them to take my word for it.

- Wrong: "Please assign this to me, I'll get started right away!"
- Right: "Here's my reproduction (below). If this looks right to you, I'd like to take this one."

### Rule: match certainty to what the artifact shows

If my repro didn't trigger the bug, I say so plainly instead of
implying success. An honest cannot-reproduce is more useful than a
confident non-finding.

- Wrong: "Can confirm, this is definitely happening." (when the artifact actually shows something adjacent, or nothing)
- Right: "I ran the exact steps and didn't see the reported behavior; here's what I got instead, and what might differ in my setup."

## Things I never post

- A request to be assigned or have an issue "reserved" without a
  reproduction already in the same comment.
- A stated root cause I have not actually traced or tested — only
  hypotheses, and only labeled as such.
- A confirmation ("can confirm", "same issue") when my own artifact
  doesn't actually show the reported behavior.

### Rule: a plan comment says what I will NOT do, and what I don't know

When I post a plan I name what is out of scope and at least one thing I haven't verified, so a maintainer isn't promised more than I can deliver. I never promise a PR date, and I disclose AI assistance if the repo asks for it.

- Wrong: "I'll have a PR up by Friday that fixes this and cleans up the surrounding code."
- Right: "Plan: change only `X` in `Y`; leaving `Z` alone. I haven't checked whether `W` also hits this path. Drafted with AI assistance (Claude Code); I read and ran everything I'm describing."

### Rule: a PR title and description promise exactly what the diff contains

The title names the actual change so a maintainer can decide in thirty seconds whether to open it. The description lists what the diff does and nothing it doesn't: I never write "exactly as planned" or "no other changes" unless I compared the diff with the plan. A shortfall or deviation is stated flat, with the reason, as a fact and not an apology.

- Wrong: "Adds a full red-team suite, fully tested and ready to merge!"
- Right: "Adds a corpus-driven red-team test and a CI job. Not done: the D-04 bypass stays an xfail. Not verified: the CI job on GitHub's runner (I ran Python 3.14 locally, CI uses 3.11)."

### Things I never put in a PR

- A ticked test box with no output behind it, or "tests pass" with nothing named.
- A description claim the diff does not contain.
