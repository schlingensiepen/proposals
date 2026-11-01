---
type: Proposal Rule
description: At the start of the service, what lies in the in-progress folder is renamed back into the waiting folder and what lies in the staging folder is deleted; there is no further restart logic.
number: F7
binding: must
role: load-bearing
applies_to: [builder]
---

# F7 · Restart is a cleanup rule, not a procedure

**Applies to:** builder.

**What holds.** At the start of the service exactly two things hold: what lies in the in-progress folder is renamed back into the waiting folder; what lies in the staging folder is deleted. There is no further restart logic.

**Why.** A job in the in-progress folder after a restart means exactly one thing: the service died while processing it. Whether it had already had an effect, the file system cannot know; that is why it is retried, and that is why the execution has to be built so that a retry is bearable. Files in the staging folder are by definition unfinished and therefore worthless. Both rules can be implemented in a few lines and can also be followed by hand in a fault.

**Obvious and rejected.** *Write a log in order to reconstruct the progress exactly*: that is the beginning of a database, and it would not have answered the same question ("had the last step already taken effect?") either. *Treat in-progress jobs as failed after a restart*: loses jobs that were merely unluckily timed.

**How you notice it is violated.** After a hard restart, jobs lie permanently in the in-progress folder and nobody touches them any more.
