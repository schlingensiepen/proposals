---
type: Proposal Choice
description: What holds when a place is set but to nothing; the default applies, or the start aborts because set-and-empty is an error of whoever set it.
number: W2
granularity: per-setting
applies_to: [builder, operator]
---

# W2 · The empty value

**The choice.** What holds when a place is set, but to nothing.
Decided per setting by the builder together with the user.

**(a) Empty is unset.** The default applies; the check shows the value
with the source "default (set empty)". Fits settings with a usable
default and little harm if wrong: port, log level.

**(b) Empty is set and invalid.** The start aborts, with the reason and
the next step; no silent fallback to the default or to a lower rank.
Fits settings without a default or with damage on a silent fallback: the
target system, the data directory, anything that is supposed to point at
a volume.

**Background.** Container platforms set variables to empty rather than
omitting them when the value to substitute is missing; the operator
believes their value is in effect.

**Rejected for all options.** *Treating empty silently as unset without
the check showing it.*

**Record your decision.** Per setting; the group "required values
without a default" is always (b).
