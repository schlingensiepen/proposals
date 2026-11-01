---
type: Proposal Rule
description: Done and failed jobs stay where they are, as a journal and as items awaiting a decision; there is no automatic expiry, only leftovers in the staging folder are cleared automatically, everything else is deleted explicitly.
number: F8
binding: must
role: independent
applies_to: [builder, operator]
---

# F8 · The service deletes no payload data

**Applies to:** builder · operator.

**What holds.** What is done and what has failed stay where they are: the one as a journal, the other as awaiting a decision. There is no automatic expiry time and no consumption semantics for payload data. Only the staging folder is cleared automatically (unfinished leftovers older than one hour). Everything else is deleted explicitly, by the caller or by a command of the operator.

**Why.** This rule was found only after a rejected first draft, and that is its most important part. The first draft assumed that uploaded files were single-use artifacts: consumed as soon as the job is done, plus an expiry time for orphans. The assumption is wrong: an attachment such as a logo, a form or a signature is uploaded once and used by any number of later jobs. With that, every "consumed" or "orphaned" heuristic is meaningless, and every expiry time is an arbitrarily set number that at some point destroys data somebody still needed. The honest rule is therefore: the pool fills up, and the operator cleans up. Filling up is a visible problem; silent deletion is not, until it is one.

**Obvious and rejected.** *Expiry times for uploads*: arbitrary, and they hit the artifacts in permanent use. *Delete after use*: misjudges the reuse. *Slim down what is done automatically*: takes from the operator exactly the journal they need in a fault. *Size-based eviction*: was considered and deliberately deferred to a later version; it is sensible, but it needs a quota rule, and that is a topic of its own.

**How you notice it is violated.** Somebody looks for a file that ought to exist and finds the explanation "removed automatically".
