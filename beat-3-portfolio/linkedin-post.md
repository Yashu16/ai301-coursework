I opened a fix for NVDA, the open source screen reader used by blind and low-vision people worldwide, adding a missing settings control that had been requested by a maintainer. NVDA could already play quiet white noise to stop certain audio drivers from clipping the end of speech, but there was no way for a regular user to turn that on without editing a config file by hand.

I added the control to NVDA's Audio settings, wrote the documentation and automated tests for it, and opened a pull request. A reviewer raised a good question about whether the setting's name would make sense to everyone, and that discussion led me to notice and fix a second, smaller problem: the control stayed active even when it had no effect, so I made it disable itself in that case and added tests proving the fix.

The pull request is still open and under review, but the back-and-forth with the maintainers has already made the change better than my first version. It's a small feature, but it's used by real screen reader users, and working through an outside reviewer's feedback on a large, long-running project was a good first experience in open source.

https://github.com/nvaccess/nvda/pull/21131

#OpenSource #Accessibility #NVDA
