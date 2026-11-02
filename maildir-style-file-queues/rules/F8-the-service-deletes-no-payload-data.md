---
type: Proposal Rule
description: Uploads in the pool are never deleted automatically; done and failed jobs stay as a journal and as items awaiting a decision unless the retention choice W1 says otherwise; beyond what W1 allows, only leftovers in the staging folder are cleared automatically, everything else is deleted explicitly.
number: F8
binding: must
role: independent
applies_to: [builder, operator]
---

# F8 · The service deletes no payload data

**Applies to:** builder · operator.

**What holds.** Uploads in the attachment pool are never deleted automatically: there is no expiry time and no consumption semantics for them. What is done and what has failed stay where they are, the one as a journal, the other as awaiting a decision; whether the journal is cleared by age or under a size quota is the choice [W1](../choices/W1-retention.md), and without a recorded decision it is not cleared. Beyond what W1 allows, only the staging folder is cleared automatically (unfinished leftovers older than one hour). Everything else is deleted explicitly, by the caller or by a command of the operator.

**Why.** This rule was found only after a rejected first draft, and that is its most important part. The first draft assumed that uploaded files were single-use artifacts: consumed as soon as the job is done, plus an expiry time for orphans. The assumption is wrong: an attachment such as a logo, a form or a signature is uploaded once and used by any number of later jobs. With that, every "consumed" or "orphaned" heuristic is meaningless, and every expiry time is an arbitrarily set number that at some point destroys data somebody still needed. The honest rule is therefore: the pool fills up, and the operator cleans up. Filling up is a visible problem; silent deletion is not, until it is one.

**Considered and rejected.** *Expiry times for uploads*: arbitrary, and they hit the artifacts in permanent use. *Delete after use*: misjudges the reuse. *Slimming down the journal automatically, and size-based eviction, as rules*: the first takes from the operator exactly the journal they need when something goes wrong, the second needs a quota rule that is a topic of its own; both are legitimate in some operations and wrong in others, which is why they are the options of W1 and not a rule.

**How you notice it is violated.** Somebody looks for a file that ought to exist and finds the explanation "removed automatically".
