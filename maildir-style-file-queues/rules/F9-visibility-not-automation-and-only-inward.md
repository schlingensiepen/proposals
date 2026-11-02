---
type: Proposal Rule
description: The state of the queue is made visible, not handled automatically, through local commands that show, retry and discard; the network surface reveals no counts, and a health endpoint says only whether the service is running.
number: F9
binding: must
role: independent
applies_to: [builder, operator]
---

# F9 · Visibility, not automation, and only inward

**Applies to:** builder · operator.

**What holds.** The state of the queue is made visible, not handled automatically: one command shows the counts per state folder and the counters, one command retries a failed job, one command discards it. This information is available **exclusively locally** over the command line; the network surface of the service reveals no counts. A health endpoint says only whether the service is running.

**Why.** Automation decides without knowing the situation; an operator with a list in front of them decides correctly. And counts are operational internals: anyone who can read from outside how much is waiting right now and how much is failing learns something about load and faults that is none of their business. The separation costs nothing, because anyone who may intervene is on the server anyway.

**Considered and rejected.** *Counts in the health endpoint*: convenient for monitoring, but it is the network surface, and it is open. *Automatically retry all failed jobs after a while*: also retries what failed for a good reason. *A web interface for the queue*: a management surface with everything attached to it.

**How you notice it is violated.** An outsider can tell from outside how many jobs are pending.
