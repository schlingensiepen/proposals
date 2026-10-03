---
type: Proposal Choice
description: What the service does when a rank is set but to nothing or to something unusable: abort with the reason, or warn and continue without that secret.
number: W2
granularity: per-setting
applies_to: [builder, operator]
---

# W2 · Set but empty or unusable

**The choice.** What the service does when a rank is set, but to
nothing or to something unusable (in the pattern: shorter than 32
characters; the threshold is deliberately coarse, it is meant to reject
typed short words, not to measure entropy; the command word of
[F5](../rules/F5-replacing-is-two-moves.md) is exempt). Decided per
secret.

**(a) Abort.** The start aborts, with the reason and the next step. No
fallback to a lower rank, no silent generation. Fits everything that
opens an access or reaches a target system: there would otherwise be two
truths about the secret, and the human would hold the wrong one; the operator
believes their value applies, and the service runs under another.

**(b) Warn and continue.** The service reports the unusable value and
runs without that secret; the connection concerned is switched off and
shown as switched off. Fits only optional connections whose absence the
service can display cleanly.

**Background.** Container platforms set variables to empty rather than
omitting them when the value to substitute is missing; "set and empty"
is then an error of whoever set it, which the service has to report
back to them, not repair on its own.

**Rejected for all options.** *Silently accepting a short value*: the
operator typed a word. *Generating and logging both*: the exception
from W1 (b) triggered twice, once for a case nobody ordered.
*Continuing without the secret without showing it*: the operator learns
of it from a caller's error.

**Record your decision.** Per secret; everything that opens an access
or reaches a target system is always (a).
