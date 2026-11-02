---
type: Proposal Rule
description: Changes of state happen only by renaming the file from one state folder into the next, changes of content by writing a new version in staging and renaming it over the old; all state folders are on the same local file system.
number: F3
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F3 · Every transition is an atomic rename

**Applies to:** builder · operator.

**What holds.** Changes of state happen exclusively by renaming the file from one state folder into the next. Changes *to the content* of a job file happen the same way: write the new version in the staging folder, then rename it over the old file. The operating precondition is that **all** state folders are on the same local file system; network file systems are excluded, because atomicity is not guaranteed there.

**Why.** `rename()` is the only widespread file system operation with a hard atomicity guarantee. Everything rests on it: the lock-free enqueue, the worker's claim, the crash safety. If you copy and delete instead, or overwrite a file in place, you have an intermediate state at once, and with it all the races back that the pattern avoids.

**Considered and rejected.** *Copy and then delete*: creates a moment in which the job exists twice; a crash in between leads to double processing. *Overwrite the file in place*: a reader can see half a file. *Lock files instead of renaming*: brings back the question of who cleans up an orphaned lock, and when. *Distribution over a network file system*: the atomicity guarantee does not hold there throughout; anyone who does it anyway builds on sand.

**How you notice it is violated.** A job is executed twice, or a reader stumbles over an incomplete file.
