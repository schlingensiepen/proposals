---
type: Proposal Agent Guide
name: immutable-decision-records
description: Self-contained guide for an agent that writes, revises, migrates or reads decision records; the two roles, the procedures, the MUST and NEVER rules per role, an acceptance checklist and common mistakes.
---

# Agent guide

## Purpose

Apply this guide when working with a project's architecture or decision records: writing a new one, revising a decision that already exists, or reading a set of records to find out what currently applies. Also apply it when a project asks you to "document this decision", or when you notice you are about to edit a decision record that is marked accepted.

This guide is complete on its own. Every rule, step and checklist item is written out in full here; the F numbers in parentheses are provenance only, and you can act correctly without opening the rule files. The reasoning, costs and limits are in the rest of this bundle.

The one idea behind all of it: **an accepted decision record is never edited again.** A change is a new record that replaces the old one completely. Everything below follows from that.

Two roles. Read the rule set for yours; if you do both in one task, both apply.

- **Author**: you are asked to record a decision, revise an existing one, or migrate a collection of decisions into individual records.
- **Reader**: you are reading a set of decision records to find out what applies before you build, review, or advise. This role matters more than it looks: superseded records outnumber current ones over time and rank higher in full-text search.

## Procedure

**Recording a new decision:**

1. Take the next unused number; do not fill a gap.
2. Write the head: status draft, today's date, empty relation lists.
3. Write the question, the options considered, the choice, the rationale, the consequences, and the rejected alternatives with their reasons.
4. Check for real secrets in examples and replace them with invented values.
5. Set the status to accepted and the date to the day of acceptance. From this moment the prose is frozen. A draft that is discarded instead gets the status rejected and keeps its number.
6. Add the record to the index.

**Revising an existing decision:**

1. Read the record being revised in full, including its rejected alternatives: one of them may be what you are about to choose, and its recorded reason still counts.
2. Take the next unused number and write a complete new record. Copy over every part that still holds; do not point at the predecessor for it.
3. List the predecessor's decision among the new record's rejected alternatives, together with the reason it failed in practice.
4. Set the new record's "supersedes" list to include the predecessor.
5. In the predecessor: set status to superseded, add the successor to its "superseded-by" list, and add a visible notice at the top of its body pointing at the successor. Touch nothing else in it.
6. Update the index: mark the predecessor as superseded by the successor, add the new record.

**Migrating a collection file into individual records:**

1. List every decision in the collection before writing anything, including revisions that were added as extra paragraphs under an earlier decision; each of those is a separate record that supersedes the one above it.
2. Count what you listed and compare against any count in the plan. If they disagree, keep every decision you found and report the discrepancy rather than dropping one.
3. Write one record per decision, preserving the original wording of rationale and rejected alternatives. Do not modernize the reasoning to match today's state.
4. Build the index and verify every chain resolves at both ends.
5. Mark the collection file as archived, with a note pointing at the new location. Do not delete it.

## Rules for the author

MUST:

1. Put each decision in its own file with its own sequential number. Never add a decision as a section inside a file that already holds other decisions. (F1)
2. Give every record a machine-readable head containing at least: status (draft / accepted / superseded / rejected), the date (the date of acceptance once the record is accepted), a list of the records it supersedes, and a list of the records that superseded it. Both relation fields are **lists**, even when they hold one entry or none: a decision can merge two earlier ones or be split into two later ones. The status rejected marks a draft that was discarded; its number stays with it. (F5, F6)
3. Name, in every record, the options that were seriously considered and state for each one why it did not win. If nothing else was ever on the table, write "no alternative considered, because …"; never leave the section out. (F7)
4. When revising a decision, write a **new** record that answers the question completely, including the parts that stay the same. Copy them over rather than pointing at the predecessor. (F3)
5. Give the new record its own title describing what **it** decides. Leave the old record's title exactly as it is; it must keep describing what it decided. (F4)
6. In the same change that creates a successor, update both ends of the relation: add the predecessor to the successor's "supersedes" list, add the successor to the predecessor's "superseded-by" list, and set the predecessor's status to superseded. (F5)
7. Put a visible supersession notice at the **top of the superseded record's body**, not only in its head block: a reader who reads only the opening lines must see that the record no longer applies. (F5, F8)
8. Update the index of all records in the same change: it must show, per record, whether it applies or is superseded and by what. If no index exists, create one. Give the index an entry note saying explicitly that superseded records are kept; otherwise their mere presence reads as validity. (F8)
9. Use invented values in every example: keys, hostnames, credentials, tokens. Once a record is accepted, a real secret inside it cannot be quietly removed. If one is found anyway, secret rotation takes precedence over immutability: rotate the key, and handle its removal from the text as an explicit, logged exception, not as a silent correction. (F2)

NEVER:

- Never edit the content of a record whose status is accepted: its statement, rationale, options, and consequences. Exactly two places stay editable, the **head** (status and the two relation lists) and the **opening of the document** (the supersession notice pointing at the successor), plus two repairs: typographical errors and dead links. Anything else is a new record. Both exceptions exist for one reason: the backward pointer comes into being later than the record it is in, so without them the replacement could only ever be recorded forwards. (F2, F5)
- Never write "partly superseded", "still applies except section 3", or "otherwise the predecessor applies". Replacement is all-or-nothing. (F3)
- Never delete, empty, rename, or move a superseded record, including moving it into an archive directory. Existing pointers to it must keep resolving. (F4)
- Never reuse a number, not even one freed by discarding a draft. Gaps in the sequence are normal; do not close them. (F6)
- Never renumber existing records to make the sequence tidy. (F6)
- Never drop a decision during a migration because a plan's count says it should not exist. When a plan's prose and its totals disagree, keep the record and report the discrepancy. (F1)

## Rules for the reader

MUST:

1. Read the index first and determine each record's status before using its content. A record's existence is not evidence that it applies. (F8)
2. Check the status and the "superseded-by" list of every record you are about to rely on, including ones you found by search rather than through the index. (F5, F8)
3. When a record is superseded, follow the chain forward to the current record and use that one. Read the superseded record only to learn what was rejected and why. (F4)
4. Treat the rejected alternatives of a current record as binding context: do not propose a solution the set of records has already rejected without addressing the recorded reason. (F7)

NEVER:

- Never treat a whole directory of decision records as uniformly valid because you loaded it into context. In the author's own set of records, nine of thirty-seven no longer applied. (F8)
- Never resolve a contradiction between two records by picking the more detailed or more recent-looking one. Resolve it by status: the current record wins, whatever its length. (F8)

## Acceptance checklist

- [ ] Does every decision live in its own file, with no file holding two decisions? (F1)
- [ ] Is every accepted record's last content change no later than its acceptance date, apart from head updates, the supersession notice at its opening, typo fixes, and dead-link repairs? (F2)
- [ ] Does every replacing record answer its question completely, without pointing at a predecessor for any part of the answer? (F3)
- [ ] Does every superseded record still carry its own original title, describing what it decided rather than what its successor decides? (F4)
- [ ] Does every superseded record still exist at its original path, unmoved and unrenamed? (F4)
- [ ] For every replacement: does the successor list the predecessor, and does the predecessor list the successor? (F5)
- [ ] Does every superseded record show its supersession at the top of its body, not only in the head block? (F5)
- [ ] Are all numbers unique across the whole set of records, including numbers freed by discarded drafts? (F6)
- [ ] Does every record name the alternatives considered, each with the reason it was rejected, or state explicitly that there were none? (F7)
- [ ] Does the index list every record with its status, and does its content match the heads of the records? (F8)
- [ ] Are all examples free of real keys, hostnames, and credentials? (F2)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Adding a "Revision of …" paragraph to an accepted record | Write a new record that answers the question completely and link both ends |
| Writing "supersedes ADR-7 in part" | Rewrite the new record so it stands alone; the predecessor is superseded as a whole or not at all |
| Renaming the old record to the new title so the index looks consistent | Restore the old title; the index shows status through its own markers, not through titles |
| Moving superseded records into `archive/` | Move them back; mark the status in the record and the index instead |
| Filling a gap in the numbering with a new decision | Use the next unused number; leave the gap |
| Listing alternatives without reasons | Add the reason for each, or the list invites the same evaluation all over again |
| Searching the set of records and using the first hit | Check its status and follow the chain forward before relying on it |
| Reporting "all decisions migrated" without recounting | Recount against the source; if a plan's total and its prose disagree, keep everything and say so |

## Self-check

Before you report completion, run the acceptance checklist against every record you wrote or touched and against the index. If a rule seems wrong for your case, read the rules and the scope of this bundle before deviating, and record the deviation.
