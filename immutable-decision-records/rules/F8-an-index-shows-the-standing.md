---
type: Proposal Rule
description: An index above all records shows, for every record, whether it applies or is superseded and by what; it is updated in the same move as every new record.
number: F8
binding: must
role: independent
applies_to: [author, reader]
---

# F8 · An index shows the status of the whole set of records

**Applies to:** author, who keeps it; reader, who reads it first.

**What holds.** An overview above all records shows, for every record, whether it applies or is superseded and by what. It is updated in the same move as every new record.

**Why.** The set of records is read by full-text search, and full-text search knows no status: it returns the superseded record just like the applicable one, often first, because the older one has existed longer and is linked more often. Without an index, the question "what actually applies?" can only be answered by opening **every** record. Nobody does that.

**Considered and rejected.** *Rely on the status marker inside the record*: necessary but not sufficient: it answers the question only once you have already found the right record. *Generate the index from the heads*: correct and recommended as soon as a tool exists; without a tool a hand-kept index beats no index, and the effort is one line per decision.

**How you notice it is violated.** Someone implements a decision that has been superseded for half a year, citing the set of records while doing so.
