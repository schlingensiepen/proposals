---
type: Proposal Rule
description: Where the tools allow it, the product repository is embedded as a submodule in a subdirectory of the work repository, which thereby records on which product state each working state was based; the alternative to F5.
number: F4
binding: should
role: independent
applies_to: [author]
---

# F4 · Submodule form: the exact reference, where the tools allow it

**Applies to:** author. Alternative to F5; the tool situation decides which one applies.

**What holds.** Where possible, the product repository is embedded as a submodule in a subdirectory of the work repository. The work repository thereby records on which state of the product each working state was based.

**Why.** The recorded reference answers a question that is otherwise unanswerable: which state of the product did this decision belong to? Whoever reads a log half a year later can check out the matching state instead of guessing. That is the only real gain over the frugal form, and it is considerable when the genesis is actually used as memory.

**Obvious and rejected.** *A tool that manages several repositories side by side*: solves the same, but brings another dependency and another configuration; disproportionate for two repositories. *Everything in one repository with subdirectories*: exactly what F1 rejects. *Noting the product state in the log instead of in the reference*: works, but is manual work and goes stale silently.

**How you notice it is violated.** The submodule reference has pointed at the same state for months although the product has moved on; then it is no longer a reference but a claim.
