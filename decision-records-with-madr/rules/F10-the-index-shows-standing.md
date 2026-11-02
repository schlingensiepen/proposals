---
type: Proposal Rule
description: An overview table lists every record with number, title, date and relation, superseded rows struck through with a pointer; updated in the same move as each record.
number: F10
binding: must
role: load-bearing
applies_to: [writer]
---

# F10 · The index is a table showing standing

**Applies to:** writer.

**What holds.** Above all records there is an overview file: one row per record with
number, title, decision date and relation. Superseded rows are marked as such —
struck through and carrying the pointer to the successor. The index is updated
in the same move as every new record.

**Why.** The body is read by full-text search, and full-text search knows no
status: it returns the superseded record just like the applicable one, often
first. Without an index, the question "what actually applies?" can only be
answered by opening **every** record — nobody does that. This puts the
replacement in three places: in both heads (F2), in the superseded record itself
(F9), and here. That is not waste but the answer to three different access
paths — tool, single opened record, overview.

**Considered and rejected.** *Rely on the status marker inside the record* —
necessary but not sufficient: it answers the question only once you have already
found the right record. *Generate the index from the heads* — correct and
recommended as soon as a tool exists; without a tool a hand-kept index beats no
index, and the effort is one line per decision. *Rely on the file system's
directory listing* — it shows number and title (F4), but no status.

**How you notice it is violated.** Someone implements a decision that has been superseded
for half a year, citing the body of records while doing so.
