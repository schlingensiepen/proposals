---
type: Proposal Rule
description: The intake writes the complete job file into the staging folder, forces it to disk and then renames it into the waiting folder; several intake threads need no coordination because each touches only its own file.
number: F4
binding: must
role: load-bearing
applies_to: [builder]
---

# F4 · Enqueue without a lock

**Applies to:** builder.

**What holds.** The intake writes the complete job file into the staging folder, forces it to be written to disk, and then renames it into the waiting folder. Several intake threads work without any coordination, because each one touches only its own file.

**Why.** Locks are the most expensive part of any queue implementation: they have to be acquired, released and cleaned up after a crash. Here they disappear without replacement, because there is no jointly written storage: the only interaction between the threads is that their finished files land in the same directory. Forcing the write before the rename is necessary so that a power failure does not leave behind a job file that exists but is empty.

**Obvious and rejected.** *Write directly into the waiting folder*: then the worker can grab a half-written file. *Do without forcing the write ("it works anyway")*: it works most of the time; the exception is a job that was confirmed and is empty.

**How you notice it is violated.** Incomplete or empty files lie in the waiting folder.
