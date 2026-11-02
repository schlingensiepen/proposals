---
type: Proposal Concept
description: The problem of handing jobs from intake to execution in a small service, the concept of the folder as the state and the atomic rename as the only transition, its load-bearing structure, and why there is no separate procedure.
---

# The concept

**In one sentence.** If you have to hand jobs from intake to execution in a small service, you store every job as a file and make the folder it is in its state; changes of state are atomic renames, and so neither a database nor locks nor restart logic of its own are needed.

**Short thesis.** As soon as a service accepts something that will be carried out later, the question of the queue arises, and the reflex is a database. For a small service that is the more expensive answer: it brings concurrency questions, a schema, a migration history and a tool that has to be started before one can even look. The maildir pattern, one file per job, the folder as the state, the atomic rename as the transition, does the same with the tools the operating system already has, can be inspected with `ls` and repaired with `mv`.

## Problem

A service accepts something it cannot or should not do right away: a message that has to be delivered, a file that is processed, a job for an external system that is not answering at the moment. Between intake and execution lies a queue. The author faced exactly this case: a service accepts drafts and files them into mailboxes over IMAP; the intake answers at once, the filing takes time, and the target server is occasionally unreachable.

With that, the usual questions arise at once. Several intake threads enqueue at the same time: how does one keep them from getting in each other's way? What happens when the service dies in the middle of processing? How does one recognize what is waiting right now and what went wrong? And how does one answer a caller who asks about the state of their job?

The reflex says: a database. It answers all four questions, and it brings four new ones: a schema that has to migrate along; concurrency semantics one has to understand; a dependency that travels in the deliverable; and a tool one has to start first in order to look when something goes wrong. For a system with millions of jobs per hour, that is the right price. For a small service, it is the price of solving a solved problem once more.

Three roles carry the problem. **The builder** has to get the handover right: concurrent, crash-safe, without double processing. **The operator** has to be able to see what is pending when something goes wrong, and to intervene without stopping the service. **The caller** wants to know what has become of their job.

## The concept

The core is a reinterpretation: **the folder is the state.** Every job is a file. Which directory it is in says where it is in the flow: accepted, waiting, in progress, done, failed. A change of state is not a write operation on a record but a rename of the file from one directory into the next.

This holds because `rename()` within one file system is **atomic**: the file is either at the old place or at the new one, never in between and never at both. Everything else follows from this by itself. Enqueuing needs no lock, because every thread writes only its own file: first completely into a staging folder, then one rename into the queue; half-finished jobs never become visible. Claiming by a worker needs no lock, because the rename itself is the claim: the worker whose rename succeeds has the job; the one whose rename fails because the file is no longer there was too slow and moves on. And the restart after a crash is a cleanup rule of two lines: what was in progress goes back into the queue, and the staging folders are emptied.

The pattern is old and proven: it is the **maildir principle** from the mail server world (`tmp`/`new`/`cur`), created to solve exactly this problem without locks across NFS boundaries. What is new here is only the transfer to the job queue of a small service, and the insight that the second great advantage is not the technique but **operability when something goes wrong**: the state of the queue is visible with `ls` and correctable with `mv`, by anybody who can use a command line, without an additional tool and without stopping the service.

The price is just as clear: everything that would be a query in a database is a directory walk here. Overviews, counts, searches by attribute: all of it costs linear time in the number of jobs. The pattern therefore has a size beyond which it no longer holds (see where it applies, in the [scope](./scope.md)).

**Load-bearing structure.** F3 carries everything: without the atomicity of the rename, F4, F5 and F7 are no longer correct, only probably right. F1 and F2 are the form in which the pattern becomes applicable at all: a job that does not fit into one file, and a state that is not in the folder, break it at once. F6 and F7 are the operational robustness, F8 and F9 the operational culture; the latter two are independent and could be decided differently without losing the pattern.

## The procedure

This concept has no procedure of its own: the order follows the path of a job, intake, enqueue, claim, retry, restart, retention, visibility. The [agent guide](./AGENT.md) lays it out as steps derived from the rules and the choice.
