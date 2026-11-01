---
type: Proposal Rule
description: A job to be retried goes back into the waiting folder with the time of the next attempt and the number of attempts in the file; only when there is no further retry does it move to the failed folder, with the last error in the file.
number: F6
binding: must
role: load-bearing
applies_to: [builder]
---

# F6 · Retry stays in the queue

**Applies to:** builder.

**What holds.** A job that fails and is to be retried goes back into the waiting folder; the time of the next attempt and the number of attempts so far are *in the file*. The worker skips jobs whose time has not yet come. Only when there is no further retry does the job move into the failed folder, with the last error in the file.

**Why.** A retry folder of its own would be one more state with a cleanup rule of its own, without anything to gain. And the retry data belong in the file and not in the file name, because they change: a change of state is a rename, a change of data is a rewrite with a rename (F3); both cleanly separated.

**Obvious and rejected.** *A retry folder of its own per attempt stage*: multiplies the states without any gain in insight. *Keep the retry in the worker's memory*: survives no restart and turns a waiting retry into a lost job. *Retry without limit*: a job that can never be delivered keeps the worker busy forever; the failed folder is the answer to that.

**How you notice it is violated.** After a restart, waiting retries have disappeared, or the worker keeps spinning on one job.
