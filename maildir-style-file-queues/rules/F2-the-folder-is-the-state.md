---
type: Proposal Rule
description: The flow has a fixed, small set of states, each one a directory; the state of a job is kept nowhere else, not in the file, not in an index, not in memory.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · The folder is the state

**Applies to:** builder.

**What holds.** The flow has a fixed, small set of states, and each is a directory. The state of a job is kept nowhere else: not in the file, not in an index, not in memory. Whoever wants to know the state looks where the file lies. What has proven itself: staging · waiting · in progress · done · failed.

**Why.** Two places for the same truth drift apart; a state field *in* the file would have to be changed together with the movement of the file, and exactly that simultaneity does not exist. The folder is moreover the only place the operating system manages atomically anyway: one gets consistency as a gift instead of building it.

**Obvious and rejected.** *A state field in the job file, all jobs in one directory*: requires locks when changing and turns every overview into reading all files. *An additional index for fast queries*: the second truth, which is wrong exactly when one needs it; whoever needs queries has left the size of this pattern (see where it applies, in the [scope](../scope.md)).

**How you notice it is violated.** There is a place in the code that determines the state of a job other than by its directory.
