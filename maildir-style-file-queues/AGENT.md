---
type: Proposal Agent Guide
name: maildir-style-file-queues
description: Self-contained guide for an agent that builds or reviews a job queue as a directory tree in a small service; the steps with the duties of adoption, the rules, an acceptance checklist and common mistakes.
---

# Agent guide

## Purpose

You are building the handover of jobs from intake to execution in a small service as a directory tree, or you are reviewing such a queue for hidden races, lost jobs, silent deletion or counts leaking to the network. This guide is complete on its own. Rules are named in parentheses as provenance. This concept has no choices; every entry is a rule.

## Procedure

**1 · Check the fit.** The pattern holds only if all of these are true: the whole tree lies on one local file system, on one machine; the stock of jobs stays at a size where a directory walk takes seconds for the operator (a few thousand to a few tens of thousands, depending on the file system); the actual work can be repeated or is secured from outside against a double effect; the jobs may lie in plain text on the disk; nobody needs regular queries by attribute. If any of these fails, stop and tell the user: the answer is real queue infrastructure or an embedded database. *Result:* a reasoned decision.

**2 · Lay out the state folders.** Five directories: staging, waiting, in progress, done, failed, all on the same local file system, no mount point inside the tree. The state of a job is where its file lies and nowhere else: no state field in the file, no index, nothing in memory. Give each access its own tree, so that a forgotten filter cannot find foreign jobs. *Result:* the directory layout. (F2, F3)

**3 · Define the job file.** One job is one file, complete, in already validated form: the intake validates and translates once, the worker never validates again. Name the file with a coarse ordering mark in front (a timestamp) and a cryptographically random identifier; never a sequential number. Attachments go into a separate, durable pool that the job refers to, not into further files of the job. *Result:* the file format and naming scheme. (F1)

**4 · Build the enqueue.** Write the complete file into staging, force it to disk, then rename it into waiting. No lock, no coordination between intake threads. Never write directly into waiting. *Result:* the enqueue path. (F4, F3)

**5 · Build the claim.** A worker takes a job by renaming it from waiting into in progress. If the rename fails because the file is gone, another worker was first: accept that silently and take the next job; it is not an error. *Result:* the worker loop. (F5)

**6 · Build retry and final failure.** A failed job that is to be retried goes back into waiting with the time of the next attempt and the number of attempts written into the file, by writing a new version in staging and renaming it over the old one. The worker skips jobs whose time has not yet come. When there is no further retry, the job moves into failed with the last error in the file. **Duty of adoption:** decide with the user which retry schedule applies and when a job counts as finally failed; this depends on the target system. Write both down. *Result:* the retry logic and the documented schedule. (F6, F3)

**7 · Build the restart rule.** At the start of the service, and nothing more: rename everything in in progress back into waiting; delete everything in staging. *Result:* the start routine. (F7)

**8 · Settle retention.** Nothing in done, failed or the pool is deleted automatically; only leftovers in staging older than one hour are cleared. Everything else is deleted explicitly, by the caller or by an operator command. *Result:* the cleanup rules. (F8)

**9 · Build the operator commands.** Local commands only: one shows stocks and counters, one retries a failed job, one discards it. The network surface reveals no counts; a health point says only whether the service is running. **Duty of adoption:** determine with the user at which stock the queue becomes unwieldy on the target file system, and what happens then; write the threshold down before it is reached. *Result:* the command set and the recorded threshold. (F9)

## Rules

MUST: one job, one complete, validated file with a random identifier (F1) · the folder is the state, kept nowhere else (F2) · every transition, including a change of content, is an atomic rename on one local file system (F3) · enqueue by writing in staging, forcing to disk, renaming into waiting (F4) · the claim is the rename into in progress, a failed claim is silently accepted (F5) · retry data in the file, retries in waiting, final failure in failed (F6) · restart: in progress back to waiting, staging emptied (F7) · journal and pool kept until explicitly deleted (F8) · counts and interventions only through local commands (F9).

NEVER: a state field or index beside the folders (F2) · copy-and-delete or overwrite in place (F3) · lock files instead of the rename (F3) · the tree on a network file system (F3) · writing directly into waiting (F4) · read-then-mark as a claim (F5) · retries kept in memory, or retried without limit (F6) · a progress log to reconstruct the restart (F7) · expiry times or delete-after-use for payload data (F8) · counts in the health point, automatic retry of all failures, a web interface for the queue (F9).

## Acceptance checklist

- [ ] Every job is exactly one file, complete and validated; identifiers are random, with a timestamp in front. (F1)
- [ ] No code determines the state of a job other than by its directory. (F2)
- [ ] All state folders lie on one local file system; every transition and every content change is a rename. (F3)
- [ ] The waiting folder never contains incomplete or empty files; the write is forced before the rename. (F4)
- [ ] Two workers can never process the same job; a failed claim is not reported as an error. (F5)
- [ ] Waiting retries survive a restart; no job keeps the worker spinning; the retry schedule and final failure are written down. (F6)
- [ ] After a hard restart no job stays in progress; staging is empty. (F7)
- [ ] Nothing but staging leftovers is removed automatically. (F8)
- [ ] Nobody can tell from outside how many jobs are pending; the threshold at which the queue becomes unwieldy is recorded. (F9)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Raw request stored, validated only in the worker | Validate once at intake; the worker reads validated data and can no longer report "invalid" to anybody (F1) |
| Header, payload and attachments as separate job files | One file per job; attachments in a durable pool the job refers to (F1, F8) |
| Sequential job numbers | Random identifiers; numbers reveal throughput and can be guessed (F1) |
| A `status` field in the job file | The folder is the state; a field would have to change together with the move, which cannot happen (F2) |
| An index for fast queries | It is a second truth; whoever needs queries has outgrown this pattern (F2) |
| Copy, then delete | Rename; a copy is a moment in which the job exists twice (F3) |
| Lock files to guard the queue | Rename; an orphaned lock raises the question of who removes it, and when (F3) |
| Writing straight into waiting, or skipping the forced write | Write in staging, force to disk, then rename; otherwise a confirmed job can be half or empty (F4) |
| Read the job, then mark it taken | Rename into in progress; between reading and marking lies the double claim (F5) |
| A retry folder per attempt stage | Retry data in the file, the job back in waiting (F6) |
| Unlimited retries | A final failure moves the job into failed (F6) |
| In-progress jobs treated as failed after a restart | Rename them back into waiting; the execution must bear a retry (F7) |
| Expiry times for uploads | The pool fills up and the operator cleans up; expiry hits the artifacts in permanent use (F8) |
| Queue counts in the health endpoint | Counts only through local commands (F9) |

## Self-check

Before you report completion: can you name the file system the whole tree lies on, the five state folders, the naming scheme of a job file, the recorded retry schedule and final failure, the restart rule, what is deleted automatically (only staging leftovers), and the recorded threshold at which the queue becomes unwieldy? If any of these is missing, you are not done.
