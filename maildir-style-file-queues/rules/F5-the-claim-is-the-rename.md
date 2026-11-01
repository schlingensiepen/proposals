---
type: Proposal Rule
description: A worker takes over a job by renaming it into the in-progress folder; if the rename succeeds the job is its own, if it fails because the file is gone another worker was first, and the failure is silently accepted.
number: F5
binding: must
role: load-bearing
applies_to: [builder]
---

# F5 · The claim is the rename

**Applies to:** builder.

**What holds.** A worker takes over a job by renaming it into the in-progress folder. If the rename succeeds, the job belongs to that worker; if it fails because the file no longer exists, another worker got it first. Then the failure is silently accepted and the next job is taken. There is no further coordination between workers.

**Why.** This is the same trick as with enqueuing, only the other way round: the atomicity of the rename already is a mutual exclusion, so one does not need to build one. The approach moreover scales unchanged from one worker to several, without anything changing in the code.

**Obvious and rejected.** *Read first, then mark as taken*: between reading and marking lies the time window for a double claim. *One worker that is the only one to know the queue*: works, but prevents any later parallelization and makes the worker the bottleneck. *A coordination layer (lock service, database transaction)*: solves a problem one does not have at this point.

**How you notice it is violated.** Two workers process the same job, or a worker treats the failure of a claim as an error and reports it.
