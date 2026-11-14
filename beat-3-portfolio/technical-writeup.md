# Exposing a hidden NVDA audio setting, and fixing the control I added for it

## Overview

NVDA, an open source screen reader, can play very quiet white noise after speech to keep a user's audio device awake, because some drivers trim the silence NVDA normally plays and that trimming clips the tail end of what NVDA says. The setting that controls that white noise's volume, `audio.whiteNoiseVolume`, already existed in NVDA's configuration and audio code, but there was no way to change it except from NVDA's Python console or by hand-editing the config file. I picked up [NVDA issue #19286](https://github.com/nvaccess/nvda/issues/19286), where a maintainer had agreed the scope was to add a real control for it in the Audio settings panel, document it, and document a Realtek-specific workaround a user had found. I opened [PR #21131](https://github.com/nvaccess/nvda/pull/21131), which is still open (draft) and has gone through one round of reviewer feedback that led me to add a second fix: the new control now disables itself whenever it would have no effect.

## What happened

I started by reproducing the gap on unmodified upstream NVDA (commit `7b58df9`): I opened Settings > Audio and confirmed the panel ended at "Time to keep audio device awake after speech (seconds)," with nothing after it, and I grepped the source to confirm the setting existed in the code but nowhere under `source/gui`:

```
source/config/configSpec.py:68:  whiteNoiseVolume = integer(default=0, min=0, max=100)
source/nvwave.py:367:            c_float(config.conf["audio"]["whiteNoiseVolume"] / 100.0),
```

I added a spin control to `AudioPanel` in `source/gui/settingsDialogs.py`, built the same way as the existing awake-time control, wrote a User Guide section with an anchor so F1 opens it, added a changes entry, and wrote three unit tests for the new control (its saved value, its range against the config spec, and that saving writes the value back). All three tests fail on unmodified `AudioPanel` and pass with my change, which is my first piece of proof on the page: the control's absence was not just a screenshot, it was a test that failed until the control existed.

After I opened the PR, a collaborator, CyrilleB79, reviewed it and raised a naming question rather than a code issue: "white noise" is not a widely known term, and they asked if anyone had a better wording. I suggested "static noise." A maintainer, seanbudd, said either was fine but flagged NVDA's policy against colorised terms, since "white" can carry other meanings elsewhere in software. CyrilleB79 settled it by explaining that "white noise" is the acoustic term for noise with equal energy across the frequency spectrum, not a colorised metaphor, and didn't need to change. That's my second piece of proof — the actual review thread, not a paraphrase:

> "white noise" is not a very known concept. So I was searching a better wording of this option, but I did not find one.
> — CyrilleB79, PR review comment

> It might also be best to avoid colorised terms per our language choices policy.
> — seanbudd, PR review comment

That discussion didn't change any code, but it did point me at something the code was still getting wrong: the control I'd added was always enabled, even when the awake-time value was 0, where white noise has no effect at all. That's the change I made because of review feedback, not my first attempt — I bound the awake-time control's existing spin event to also enable or disable the white noise control, both when the panel opens and live as the value changes, and I added three more tests for it (disabled on open at zero, enabled on open above zero, toggles live). Those three new tests fail without the fix and pass with it; the three original tests are unaffected by it, since they test the value, range, and save behavior the fix didn't touch. I was careful in my own PR description not to call this a new feature — the control already existed and already had no effect at zero, so this closes a gap in its behavior rather than adding one.

The result that landed: the naming question that opened the review thread is resolved (we're keeping "white noise"), and my disable/enable fix is now part of the PR after that same round of review, with `rununittests`, `runlint`, `runcheckpot`, and `runlicensecheck` all passing on the branch. The PR is still open and in draft, and NVDA's CI is waiting on a maintainer to approve the first run.

## Limitations and what's next

I have not run a controlled test on the Realtek hardware the original issue reporter used, so I can't personally confirm the white noise stops the clipped speech that motivated the issue — only that the control to enable it now exists and does what it says. There's also no live preview of the volume in the panel; a change only takes effect after OK or Apply, which matches the existing awake-time control but means a user has to apply and re-test to hear the effect. Next, I plan to take the PR out of draft once CI runs cleanly and a maintainer confirms there's nothing else open, and if the Realtek reporter or another user replies, I'd like to ask them to confirm the fix on their own hardware, since that's evidence I can't produce myself.
