---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, what version 1.0 settles and what is still open.
* [Concept](./concept.md) - The problem, the concept of the folder as the state with its load-bearing structure, and why there is no separate procedure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. The pattern itself F1 to F3, concurrency F4 to F6, restart F7, operation F8 to F9.

* [F1 One job, one file](./rules/F1-one-job-one-file.md) - Every job is completely in one file in already validated form; the name carries a random identifier and a coarse ordering prefix.
* [F2 The folder is the state](./rules/F2-the-folder-is-the-state.md) - A fixed, small set of states, each a directory; the state of a job is kept nowhere else.
* [F3 Every transition is an atomic rename](./rules/F3-every-transition-is-an-atomic-rename.md) - State and content changes happen only by rename; all state folders are on one local file system.
* [F4 Enqueue without a lock](./rules/F4-enqueue-without-a-lock.md) - Write the complete file in staging, force it to disk, rename it into waiting; no coordination between intake threads.
* [F5 The claim is the rename](./rules/F5-the-claim-is-the-rename.md) - A worker claims a job by renaming it into in progress; a failed rename means another was first and is silently accepted.
* [F6 Retry stays in the queue](./rules/F6-retry-stays-in-the-queue.md) - Retry time and attempt count in the file, the job back in waiting; after the last attempt into failed.
* [F7 Restart is a cleanup rule, not a procedure](./rules/F7-restart-is-a-cleanup-rule-not-a-procedure.md) - At start, in progress goes back to waiting and staging is emptied; nothing more.
* [F8 The service deletes no payload data](./rules/F8-the-service-deletes-no-payload-data.md) - Uploads are never deleted automatically; the journal stays unless W1 says otherwise; beyond what W1 allows, only staging leftovers are cleared automatically.
* [F9 Visibility, not automation, and only inward](./rules/F9-visibility-not-automation-and-only-inward.md) - Local commands show, retry and discard; the network surface reveals no counts.

# Choices

More than one legitimate answer; decided per service by whoever adopts the concept, and recorded.

* [W1 Retention of the journal and the disk](./choices/W1-retention.md) - Nothing automatic; a retention period for the journal; a size quota; or both. Uploads are excluded in every option.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs including the price of each retention option, where it applies, when this no longer holds, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - The origin in the maildir format, the reference implementation (state open), the database that lost the comparison.
* [Sources](./sources.md) - What this proposal builds on: external sources, not yet checked.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent building or reviewing such a queue: the steps with the duties of adoption, the step of deciding the choice with the user, rules, acceptance checklist, common mistakes.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
