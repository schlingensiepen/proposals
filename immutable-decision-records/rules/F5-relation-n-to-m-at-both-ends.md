---
type: Proposal Rule
description: Every record carries two lists in its head, what it supersedes and what superseded it; both are maintained in the same move that creates the successor.
number: F5
binding: must
role: load-bearing
applies_to: [author]
---

# F5 · The replacement relation is n:m and is kept at both ends

**Applies to:** author.

**What holds.** Every record carries two **lists** in its head: what it supersedes and what superseded it. Both are maintained in the same move that creates the successor.

**Why.** The relation is not one-to-one. A revision can merge two old decisions into one, and a growing decision can split into two; both occur once a set of records is a few years old. A single-valued field would force a false entry in those cases. And both directions are needed because both reading directions occur: forwards ("what became of this decision?") and backwards ("what did this record replace, and why was that not enough?").

**Considered and rejected.** *Only the forward pointer in the new record*: then a superseded record is not recognizable as superseded on its own, and that is precisely the record a reader hits first when searching the set of records. *Single value instead of list*: forbids merging and splitting; anyone who needs them later builds an ad-hoc note in the prose and has thereby put the relation somewhere no tool will find it.

**How you notice it is violated.** A record names its successor and the successor does not name it, or the other way round.
