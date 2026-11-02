---
type: Proposal Evidence
description: Where the pattern has held; its origin in the maildir format of the mail server world, the author's draft-filing service as a reference implementation whose completion report is outstanding, and the embedded database that lost the comparison.
---

# Evidence

## Origin: the maildir format

The pattern is a transfer of the maildir format from the mail server world, created at the end of the 1990s as a lock-free alternative to the mbox format: a message is a file, it comes into being in `tmp`, is renamed to `new` and later to `cur`, exactly the three roles that are called staging, waiting and done here. That the procedure has worked under load for decades is the actual evidence of its viability; this concept only transfers it to the job queue of a small service and adds the operating rules.

## Reference implementation (state open)

The author's draft-filing service implements the pattern completely: one directory tree per access with the five state folders, a random identifier with a timestamp in front, retry with the time in the file, the restart rule at start, a journal without automatic cleanup, display of counts and interventions exclusively through commands. It is supplemented by two things that belong to the pattern but go beyond the pure queue: a separate, durable file pool for attachments (which jobs only refer to, F8), and idempotency markers that, by exclusive creation, prevent a request submitted repeatedly from producing two jobs.

*The implementation was running when this version was written; a completion report is outstanding.* Until it exists, this section claims no more than that. The repository of the service is not publicly accessible.

## Considered and found inferior

Before the decision, an embedded database was under discussion. Its only argument of its own was the status query for the caller ("what has become of my job?"), and that works just as well through the location of the folder. Against it stood: concurrency behavior one has to understand and test; a schema with a migration history; a dependency in the deliverable; and the loss of inspectability with the tools of the operating system. The decision therefore went against any database in the system.
