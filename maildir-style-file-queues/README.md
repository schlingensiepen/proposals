---
type: Proposal
description: A directory tree is the queue; every job is a file, its state is the folder it lies in, and every change of state is an atomic rename on the same file system, lock-free, crash-safe and inspectable with the tools of the operating system.
version: "0.9"
date: 2026-08-15
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# The directory tree as a queue

This directory is an OKF bundle for one proposal of the series: how a small service hands jobs over from intake to execution between concurrent parts, without a database and without locks. It is a base concept that other proposals import.

**Question.** How do I hand jobs over between concurrent parts of a small service, without a database and without locks?

**Thesis.** A directory tree is the queue: every job is a file, its state is the folder it lies in, and every change of state is an atomic rename on the same file system. That is lock-free, crash-safe, and inspectable and repairable with the tools of the operating system.

**Version 0.9** is a material version: complete in content, not yet reviewed by the author. The completion report of the reference implementation is outstanding, and the external sources are named but not yet checked against the originals; the scope lists what is still open.

## Start here

The [concept](./concept.md) describes the problem, the folder as the state with its load-bearing structure, and why there is no separate procedure; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Nine rules: the pattern itself F1 to F3, concurrency F4 to F6, restart F7, operation F8 and F9.

* [F1 One job, one file](./rules/F1-one-job-one-file.md)
* [F2 The folder is the state](./rules/F2-the-folder-is-the-state.md)
* [F3 Every transition is an atomic rename](./rules/F3-every-transition-is-an-atomic-rename.md)
* [F4 Enqueue without a lock](./rules/F4-enqueue-without-a-lock.md)
* [F5 The claim is the rename](./rules/F5-the-claim-is-the-rename.md)
* [F6 Retry stays in the queue](./rules/F6-retry-stays-in-the-queue.md)
* [F7 Restart is a cleanup rule, not a procedure](./rules/F7-restart-is-a-cleanup-rule-not-a-procedure.md)
* [F8 The service deletes no payload data](./rules/F8-the-service-deletes-no-payload-data.md)
* [F9 Visibility, not automation, and only inward](./rules/F9-visibility-not-automation-and-only-inward.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when the answer flips, what it does not solve and what is not yet settled; the [evidence](./evidence.md) gives the origin in the maildir format, the reference implementation and the database that lost the comparison. The [sources](./sources.md) name the external sources, not yet checked against the originals; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) gives the steps with the duties of adoption for building or reviewing such a queue; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
