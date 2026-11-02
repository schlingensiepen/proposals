---
type: Proposal Rule
description: A new state points to the same attachments as its predecessor as far as they still apply; no second file is created.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · Attachments are carried over, not copied

**What holds.** A new state points to the same attachments as its predecessor, as far as they still apply. No second file is created.

**Why.** Otherwise one changed line of text doubles the entire set of attachments. That is the actual reason why immutable histories are often given up: not the number of records, but the number of files.

**Considered and rejected.** *Copying attachments, "then every state is self-sufficient"*: a self-sufficiency nobody needs, at the price of unbounded growth. *Keeping attachments on the item instead of on the state*: then it is no longer recognizable which attachment held when.

**How you notice it is violated.** The number of files grows with the number of text changes.
