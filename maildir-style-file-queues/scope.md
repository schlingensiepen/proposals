---
type: Proposal Scope
description: Security considerations, what the pattern costs including the price of each retention option, where it applies, when this no longer holds, what it does not solve, and what is not yet settled.
---

# Scope

## Security considerations

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Access to someone else's jobs through guessed identifiers | A caller tries job identifiers one after another | Identifiers are cryptographically random, not sequential (F1); in addition physical separation: one directory tree of its own per access, so that a forgotten filter does not even find someone else's jobs |
| Information about the existence of someone else's jobs | The response behavior reveals whether an identifier exists | Someone else's and unknown identifiers are treated alike; the directory separation makes that the normal case instead of a special check |
| Half-written jobs are processed | Crash during intake | Staging folder plus forcing the write before the rename; unfinished files are never visible in the flow (F4) |
| Double processing | Two workers grab the same job | The claim is the rename; only one can win (F5) |
| Lost jobs after a crash | The service dies with jobs in progress | The restart rule renames them back (F7); the execution has to tolerate a retry |
| Disclosure of operational internals | Counts over the network surface | Information only locally through commands (F9) |
| Disk filling up | Uploads are never deleted automatically (F8); under W1 (a) the journal is not either | Deliberately accepted and made visible: a command shows the counts; clearing is an operator decision. Anyone who cannot accept that chooses a retention period or a quota under [W1](./choices/W1-retention.md), which still never touches uploads |
| Third-party access to the disk | Anyone who may read the tree reads all jobs | File permissions are the only protective layer; there is no encryption at rest; that is a deliberate limit (see when this no longer holds) |

Limit of the model: the queue is as well protected as the directory it is in. Anyone with read permission on the tree sees all jobs in plain text. The pattern does not replace access control, it presupposes it.

## What it costs

**Every overview is a directory walk (F2).** There is no index. "How many jobs are waiting?", "Which have failed?", "Which belong to this caller?": all of that means reading a directory, in the worst case with every file in it. With hundreds or a few thousand jobs this goes unnoticed; with hundreds of thousands it becomes a problem, and first of all for the operator who wants to look.

**The file system becomes an operating precondition (F3).** Everything has to be on one local file system. No network file system, no distribution over machines, no falling back on an object store. If you want to scale horizontally later, you cannot take this queue along.

**Many small files (F1).** Every job is a file, every retry a rewrite. That consumes inodes and makes backups slower than a single database file, and directories with very many entries are a performance problem in themselves on some file systems.

**Somebody has to clean up (F8, W1).** Uploads never disappear by themselves: the pool grows, and clearing it is a recurring operator task. For the journal and the disk, each option of [W1](./choices/W1-retention.md) has its price: (a) nothing automatic, so the journal also grows and the disk fills up at some point, visibly; (b) a retention period, so the journal is missing after the period when something goes wrong; (c) a quota, so somebody has to set and maintain an operating value, and the oldest done jobs disappear when it is exceeded; (d) both prices.

**No event stream, no priorities (F2, F6).** The queue can only do "roughly in order". Jumping the queue, priority classes, delayed delivery over long periods, separate lanes per caller: all of that would have to be encoded into folder names and file names, and from the second such wish on, you are rebuilding a database.

**No encryption at rest (see the security considerations).** The jobs are in plain text. If that is not allowed, you need a different store, and with it a different pattern.

## Where it applies

### Preconditions

**One local file system for the whole tree.** Staging and state folders have to be on the same file system, otherwise the rename is no longer a rename but a copy with a delete (F3). You recognize the situation by a mount point in the middle of the tree.

**A mounted volume with ordinary rename semantics.** In a container, the tree has to be on a mounted volume, not on the layered file system of the image; on a file system with different rename semantics, the atomicity that F3 rests on is not given.

**One service process owns the tree.** The restart rule (F7) runs at the start of the service and the cleanup of staging leftovers (F8) runs during operation; both are safe because exactly one service process, with its intake threads and workers, works on the tree. A second process starting on the same tree would clear what the first is still writing.

**A size at which walks stay cheap.** Rule of thumb: as long as a directory walk stays in the range of seconds for the operator, the pattern holds. Depending on the file system, that is a few thousand to a few tens of thousands of jobs present at once, not jobs per hour.

**An execution that tolerates a retry.** Because the restart retries in case of doubt (F7), the actual work has to be either repeatable or secured from outside against a double effect.

### When this no longer holds

**Several machines are to serve the same queue.** Then F3 falls, and with it the whole pattern. Network file systems do not guarantee atomicity throughout; anyone who has to distribute needs real queue infrastructure.

**Queries by attribute become everyday business.** As soon as "all jobs with property X" is asked regularly, an index is missing, and an index beside the directory tree is the second truth that F2 avoids. That is the point at which an embedded database becomes the more honest answer.

**Throughput reaches the limit of the file system.** At very high rates, creating and renaming many small files becomes the bottleneck itself, and directories with very many entries become slow.

**The jobs must not be in plain text.** Encryption at rest is not provided for in this pattern; retrofitting it would mean encrypting and decrypting every file individually and losing inspectability, that is, exactly the second main advantage.

### What it does not solve

**Access control.** The pattern presupposes that the file permissions are right; it does not establish them.

**Exactly-once execution.** After a crash, the job is retried without knowing whether the previous execution had already taken effect. If you need exactly-once, you need an agreement with the target system; the pattern delivers at-least-once.

**Priorities, lanes, schedules.** Everything that goes beyond "roughly in order, with retry".

**Backup.** How the tree is backed up is operational work outside the pattern. How long the journal is kept is not outside it: that is the choice [W1](./choices/W1-retention.md).

### Not yet settled

* **Backup**: a consistent snapshot of a tree in which renames are happening is not a matter of course. Possibly a note suffices that a snapshot taken during operation can contain jobs twice or not at all.
* **State of this version.** Version 1.0 settles retention as a choice; the completion report of the reference implementation is still outstanding (see the [evidence](./evidence.md)), and the external sources are named but not yet checked against the originals (see the [sources](./sources.md)).
