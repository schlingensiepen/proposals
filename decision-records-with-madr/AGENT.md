---
type: Proposal Agent Guide
name: decision-records-with-madr
description: Self-contained guide for an agent that creates, formats or reviews decision records in MADR 4.0.0 with the two replacement lists; the rules, three procedures, an acceptance checklist, common mistakes and a self-check.
---

# Agent guide

<!-- SELF-CONTAINMENT RULE: This file is handed to agents on its own. Every rule, step,
     and checklist item is written out in full HERE. The F numbers in parentheses are
     provenance markers for maintainers only; an agent must be able to act correctly
     without ever opening another file of this bundle. -->

## Purpose

Apply this guide when creating, formatting or reviewing architecture decision records
(ADRs) in a project: choosing the file format, writing the YAML head, naming the file,
recording that one decision supersedes another, or checking an existing body for format
consistency. It is distilled from the rules of this bundle and gives orders only; the
reasoning, costs and limits are in the rest of the bundle, and nothing here requires
reading them.

The format is MADR 4.0.0, adopted unchanged, with exactly one extension: two list fields
in the head recording replacement in both directions.

One premise everything below assumes, stated here so you never need to look it up: **once
a record is accepted, its content is immutable**: statement, rationale, options and
consequences are not edited again, because otherwise it is no longer datable what applied
when. Exactly two places stay changeable: the head (status and relation lists) and the
opening of the document (the supersession notice). Plus typographical errors and dead
links. A revision is a new record, not an edit.

### When to apply

- You are asked to write a decision record, or to set up decision records in a project
  that has none.
- You are revising a decision and must record that the new document supersedes an old one.
- You are reviewing or migrating an existing body of records for format consistency.

If a project already uses an ADR tool that enforces its own format, that tool wins; use
this guide to decide which fields to reproduce inside it, not to fight it.

The copyable template is in `examples/` of this bundle (English and German); if you have
only this file, item 9 of the MUST rules below tells you what the template must contain.

## Rules (MUST / NEVER)


MUST:

1. Write each record as a Markdown file following MADR 4.0.0: a YAML head between two
   lines of three hyphens, then the prose sections. Do not rename, reorder or "adapt" the
   format. (F1)
2. Put exactly these keys in the head, with the English key names unchanged: `status`
   (draft | accepted | superseded), `date` (the day it was decided), `decision-makers`,
   `consulted`, `informed`, `supersedes`, `superseded-by`. (F1, F3, F7)
3. Write `supersedes` and `superseded-by` as YAML **lists** in every record, including
   when they are empty or hold one entry — `supersedes: []`, `supersedes: [0011]`. A
   revision can merge several decisions, and one decision can split into several. (F2)
4. Record a replacement in both heads in the same change: add the predecessor's number to
   the successor's `supersedes`, add the successor's number to the predecessor's
   `superseded-by`, and set the predecessor's `status` to superseded. (F2)
5. Name files `<nnnn>-<short-title>.md`: four-digit running number, hyphen, short title in
   lowercase with hyphens, ASCII only (write out non-ASCII characters). Never reuse a
   number, not even one freed by a discarded draft; leave gaps in the sequence open. A
   reused number silently redirects every earlier citation to the wrong record. (F4)
6. Give every record these five sections, in this order: **Context and problem** ·
   **Considered options** · **Decision** (with rationale and trade-off) ·
   **Consequences** · **Rejected and why**. (F5)
7. List the options twice: briefly in "Considered options" with the chosen one marked, and
   with a spelled-out reason per rejected option in "Rejected and why". Every option in
   the list must reappear in the justification section. (F6)
8. Write the prose in one language and use that same language for the entire body. Keep
   the head keys English regardless. (F7)
9. Create a template file in the body if none exists, containing every head field and
   every section with embedded notes, and create new records by copying it. (F8)
10. Use invented values in every example — keys, hostnames, paths, credentials. An
    accepted record cannot be quietly corrected later. (F3)
11. Prefer roles over personal names in `decision-makers`, `consulted` and `informed`, or
    obtain consent: in an immutable body the name stays permanently, attached to a
    statement about responsibility. (F3)
12. When superseding a record, put a visible notice below its heading and before its first
    section, pointing at the successor. The head alone is not enough: whoever opens a
    record reads the first lines of the text, not the metadata block. (F9)
13. Keep an index above all records — one row per record with number, title, decision date
    and relation, superseded rows marked as such with a pointer to the successor. Update it
    in the same change as the record. Create it if none exists. (F10)

NEVER:

- Never invent a format of your own, and never modify MADR's section structure or head key
  names. (F1)
- Never write `superseded-by: 0031` as a plain string; it is a list. (F2)
- Never record a replacement only in the prose, or only in the index. It belongs in both
  heads. (F2)
- Never put content into the head — no summary, no rationale, no decision. The head is
  bookkeeping; it is also the only part that may change after acceptance, and content
  there would void that boundary. (F3)
- Never rename a file after its record is accepted, and never reuse a number. Gaps in the
  numbering are normal; leave them. (F4)
- Never edit the content of an accepted record — statement, rationale, options,
  consequences. A revision is a new record. (F3, F9)
- Never shorten, empty or replace the text of a superseded record. It is the only place
  holding what the decision was and why it seemed right at the time. (F9)
- Never drop a section because there is nothing to say. Write "omitted, because …". (F5)
- Never create a new record by copying the most recently written one — copy the template.
  (F8)

## Procedure

**Writing a new record:**

1. Copy the template to `<nnnn>-<short-title>.md`, taking the next unused number.
2. Fill the head: `status: draft`, today's date, participants as roles, both relation
   lists empty.
3. Write context and problem in the language of whoever had the problem — the trigger,
   not a topic announcement.
4. List the considered options briefly and mark the chosen one.
5. Write the decision as one quotable indicative sentence, then the rationale, then the
   trade-off — what spoke for the other side and why it was not enough.
6. Write the consequences, positive and negative. Do not skip the negative ones.
7. Write one paragraph per rejected option with the reason spelled out.
8. Check every example for real keys, hostnames or paths and replace them.
9. Set `status: accepted` and add the record to the index.

**Recording a replacement:**

1. Write the new record completely as above; it must answer the question on its own.
2. Set `supersedes: [<old number>]` in the new record's head.
3. In the old record: set `status: superseded`, add `superseded-by: [<new number>]`, and
   place a visible notice above the first section pointing at the successor. Change
   nothing else in it — not the title, not the prose, not the file name.
4. Update the index: mark the old record as superseded by the new one and add the new row.

**Reviewing an existing body:**

1. Check that every file name matches `<nnnn>-<short-title>.md` and that all numbers are
   unique.
2. Check that every head parses and carries all seven keys, with both relation fields as
   lists.
3. For every `supersedes` entry, verify the named record carries the matching
   `superseded-by`, and vice versa. Report every one-sided link.
4. Check that all five sections exist in every record, in order.
5. Check that every option listed in "Considered options" reappears in "Rejected and why"
   or is marked as chosen.
6. Report head fields that contain content rather than bookkeeping.

## Checklists

### Acceptance checklist

- [ ] Does every record parse as MADR 4.0.0 — YAML head plus the prescribed sections, with
      unchanged English key names? (F1, F7)
- [ ] Are `supersedes` and `superseded-by` lists in every record, even when empty? (F2)
- [ ] For every replacement: does the successor name the predecessor and the predecessor
      name the successor? (F2)
- [ ] Does the head contain bookkeeping only — no summary, no rationale, no decision? (F3)
- [ ] Do all file names match `<nnnn>-<short-title>.md`, four digits, lowercase, ASCII
      only, with unique numbers and no renamed accepted records? (F4)
- [ ] Does every record have all five sections in the prescribed order? (F5)
- [ ] Does every option in "Considered options" reappear in "Rejected and why" or as the
      chosen one? (F6)
- [ ] Is the prose in one language across the whole body? (F7)
- [ ] Does a template file exist, and does it carry every head field the records carry?
      (F8)
- [ ] Are all examples free of real keys, hostnames, paths and credentials? (F3)
- [ ] Do `decision-makers`, `consulted` and `informed` use roles, or were names entered
      deliberately? (F3)
- [ ] Does every superseded record carry a visible notice below its heading, before its
      first section, pointing at the successor? (F9)
- [ ] Is the prose of every accepted record unchanged since acceptance, apart from the
      head, that notice, typo fixes and dead-link repairs? (F3, F9)
- [ ] Does an index exist listing every record with its standing, and does it agree with
      the heads? (F10)

### Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| `superseded-by: 0031` as a plain string | Make it a list: `superseded-by: [0031]` |
| Replacement recorded only in the new record | Add `superseded-by` and `status: superseded` to the old one as well |
| A one-line summary added to the head | Remove it; the head is bookkeeping. The title carries the short form |
| New record built by copying the last one | Copy the template instead; the last record carries its own idiosyncrasies |
| A section dropped because it was empty | Restore it with "omitted, because …" |
| Options listed but never justified | Add one paragraph per rejected option with the reason spelled out |
| File renamed after the title was sharpened | Restore the original name; every existing pointer depends on it |
| Numbering gap filled with a new decision | Use the next unused number; leave the gap |
| Real hostname or key in an example | Replace before acceptance — afterwards it cannot be removed quietly |
| Supersession recorded in the head only | Add the visible notice below the heading; the head is read by tools, the notice by people |
| Superseded record emptied or reduced to a pointer | Restore its text; it is the only evidence of what was decided and why |
| Index not updated with the new record | Update it in the same change; full-text search knows no status |

## Self-check

Before you report completion: can you name the template every record was copied from,
the language of the prose, every replacement with both of its heads and its notice, and
the index row of every record? Have you run the acceptance checklist above against every
record you touched? If any of these is missing, you are not done.
