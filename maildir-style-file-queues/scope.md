---
type: Proposal Scope
description: Security considerations, what the pattern costs, where it applies, when the answer flips, what it does not solve, and what is not yet settled in this material version.
---

# Scope

## Security considerations

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Foreign access through guessed identifiers | A caller tries job identifiers one after another | Identifiers are cryptographically random, not sequential (F1); in addition physical separation: one directory tree of its own per access, so that a forgotten filter does not even find foreign jobs |
| Information about foreign existence | The response behavior reveals whether an identifier exists | Foreign and unknown identifiers are treated alike; the directory separation makes that the normal case instead of a special check |
| Half-written jobs are processed | Crash during intake | Staging folder plus forcing the write before the rename; unfinished files are never visible in the flow (F4) |
| Double processing | Two workers grab the same job | The claim is the rename; only one can win (F5) |
| Lost jobs after a crash | The service dies with jobs in progress | The restart rule renames them back (F7); the execution has to bear a retry |
| Disclosure of operational internals | Counts over the network surface | Information only locally through commands (F9) |
| Disk filling up | Payload data are never deleted automatically (F8) | Deliberately accepted and made visible: a command shows the stocks; clearing is an operator decision. Whoever cannot bear that needs the deferred quota rule (not yet settled, below) |
| Foreign access to the disk | Whoever may read the tree reads all jobs | File permissions are the only protective layer; there is no encryption at rest; that is a deliberate limit (see when the answer flips) |

Limit of the model: the queue is as well protected as the directory it lies in. Whoever has read permission on the tree sees all jobs in plain text. The pattern does not replace access control, it presupposes it.

## What it costs

**Every overview is a directory walk (F2).** There is no index. "How many jobs are waiting?", "Which have failed?", "Which belong to this caller?": all of that means reading a directory, in the unfavorable case with every file in it. With hundreds or a few thousand jobs this goes unnoticed; with hundreds of thousands it becomes a problem, and first of all for the operator who wants to look.

**The file system becomes an operating precondition (F3).** Everything has to lie on one local file system. No network file system, no distribution over machines, no falling back on an object store. Whoever wants to scale horizontally later cannot take this queue along.

**Many small files (F1).** Every job is a file, every retry a rewrite. That consumes inodes and makes backups slower than a single database file, and directories with very many entries are a performance problem in themselves on some file systems.

**The operator has to clean up (F8).** Nothing disappears by itself. The pool grows, the journal grows, and at some point the disk fills up. That is intended, but it is a recurring task somebody has to take on.

**No event stream, no priorities (F2, F6).** The queue can only do "roughly in order". Jumping the queue, priority classes, delayed delivery over long periods, compartments per caller: all of that would have to be encoded into folder names and file names, and from the second such wish on one is rebuilding a database.

**No encryption at rest (see the security considerations).** The jobs lie in plain text. Whoever is not allowed that needs a different store, and with it a different pattern.

## Where it applies

### Preconditions

**One local file system for the whole tree.** Staging and state folders have to lie on the same file system, otherwise the rename is no longer a rename but a copy with a delete (F3). You recognize the situation by a mount point in the middle of the tree.

**A size at which walks stay cheap.** Rule of thumb: as long as a directory walk stays in the range of seconds for the operator, the pattern holds. Depending on the file system, that is a few thousand to a few tens of thousands of jobs in stock, not in throughput.

**An execution that bears a retry.** Because the restart retries in case of doubt (F7), the actual work has to be either repeatable or secured from outside against a double effect.

### When the answer flips

**Several machines are to serve the same queue.** Then F3 falls, and with it the whole pattern. Network file systems do not guarantee atomicity throughout; whoever has to distribute needs real queue infrastructure.

**Queries by attribute become everyday business.** As soon as "all jobs with property X" is asked regularly, an index is missing, and an index beside the directory tree is the second truth that F2 avoids. That is the point at which an embedded database becomes the more honest answer.

**Throughput reaches the limit of the file system.** At very high rates, creating and renaming many small files becomes the bottleneck itself, and directories with very many entries become slow.

**The jobs must not lie in plain text.** Encryption at rest is not provided for in this pattern; retrofitting it would mean encrypting and decrypting every file individually and losing inspectability, that is, exactly the second main advantage.

### What it does not solve

**Access control.** The pattern presupposes that the file permissions are right; it does not establish them.

**Exactly-once execution.** After a crash, the job is retried without knowing whether the previous execution had already taken effect. Whoever needs exactly-once needs an agreement with the target system; the pattern delivers at-least-once.

**Priorities, compartments, schedules.** Everything that goes beyond "roughly in order, with retry".

**Retention and backup.** How long the journal is kept and how the tree is backed up is operational work outside the pattern.

### Not yet settled

* **Quotas and size-based eviction** were deliberately deferred (F8). Whether they should be added as a rule, and if so, per access or per service, by age or by size, is open.
* **The retention period of the journal**: whether it belongs in this pattern or in operations. Without a rule the journal grows without limit; with a rule it contradicts F8 to the letter.
* **Backup**: a consistent snapshot of a tree in which renames are happening is not a matter of course. Possibly a note suffices that a snapshot taken during operation can contain jobs twice or not at all.
* **File systems with different rename semantics, and containers with layered file systems**: the expectation is that a volume has to be mounted for the tree; an explicit statement is missing.
* **State of this version.** This is a material version 0.9: complete in content, not yet reviewed by the author. The completion report of the reference implementation is outstanding (see the [evidence](./evidence.md)), and the external sources are named but not yet checked against the originals (see the [sources](./sources.md)).
