---
type: Proposal Rule
description: A start secret is obtained from the environment, then from a pass-through file, then, only where W1 allows it, over the once-only channel; further sources per W3; the same ranking for every secret and every role, in one place in the code.
number: F1
binding: must
role: load-bearing
applies_to: [builder, consumer]
---

# F1 · The ranking is fixed and named

**What holds.** A start secret is obtained in this ranking: **1.** from
the environment; **2.** from a file that only passes through; **3.**,
only where [W1 (b)](../choices/W1-no-secret-set.md) is chosen, over the
once-only channel. Further sources (a platform's secret manager) are,
per [W3](../choices/W3-further-sources.md), either a rank of their own
between 2 and 3 or a feeder to 1 or 2. The ranking is the same for every
start secret and every role, and it lives in one place in the code.

**Why.** A ranking that differs per secret cannot be checked or
documented by anyone; one that differs per role forces the agent to
learn something other than the operator. Environment first, because it
is the way a platform with its own secret management hands the secret in
without it ever touching a disk or a log. The file next, because not
every platform can set a variable before the start. The once-only
channel last, because it is the exception.

**Considered and rejected.** *Only the environment*: then every platform
must be able to set a variable before the start; a platform that cannot
has no way in.
*Only the file*: then there is no way out of a secret manager without
the secret ever being on disk. *A ranking of its own per secret*:
uncheckable.

**How you notice it is violated.** The documentation explains per
secret, differently each time, where it comes from; or the code asks
the sources in three places in three orders.
