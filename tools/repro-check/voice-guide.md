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
