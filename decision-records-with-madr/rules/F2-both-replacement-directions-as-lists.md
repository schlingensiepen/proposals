---
type: Proposal Rule
description: The head carries supersedes and superseded-by, always as lists, so that a record found on its own can say it was superseded and merges and splits stay expressible.
number: F2
binding: must
role: load-bearing
applies_to: [writer]
---

# F2 · The only addition to the head: both replacement directions as lists

**Applies to:** writer.

**What holds.** The head carries two additional fields, `supersedes` and
`superseded-by`, always as a **list** — even when empty or holding a single
entry.

**Why.** MADR knows a "superseded" status, but the relation itself is not
properly stored in the format; it usually ends up as a sentence in the prose.
There no tool finds it, and on reading you only see it if you read in the right
place. As a head field it is queryable — and **as a list**, because the relation
is n:m: a revision can merge two old decisions, and a grown decision can split
into two. Both occur once a body is a few years old; a single value would force a
white lie in those cases. That it is **both** directions has a practical reason:
anyone who searches the body hits the older record first, because it has existed
longer and is linked more often — it must be able to say on its own that it was
superseded.

**Considered and rejected.** *Only `superseded-by`, resolving the reverse direction
by search* — works while the body is small and fails silently as soon as someone
renames a file. *Keeping the relation only in the index* — then a record picked
up on its own can no longer be judged, and that is exactly how records are read:
individually, out of a search. *Single values instead of lists* — forbids merging
and splitting, and anyone who needs them later writes an ad-hoc note into the prose.

**How you notice it is violated.** A head carries `superseded-by` as a plain string
instead of a list — or the replacement lives only in the prose.
