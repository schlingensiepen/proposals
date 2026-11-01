---
type: Proposal Rule
description: Genesis and product lie in separate repositories; the work history is a diary and may be untidy, the product history is a publication history; neither is smoothed nor bloated.
number: F1
binding: must
role: load-bearing
applies_to: [author]
---

# F1 · Two repositories, two histories

**Applies to:** author.

**What holds.** Genesis and product lie in separate repositories. The history of the work repository is a diary and may be untidy; the history of the product repository is a publication history and is kept as one. Neither is the one smoothed nor the other bloated.

**Why.** The two histories have different readers and different purposes, and every attempt to unite them in one sacrifices one of them. Smoothing the genesis destroys the reasons and the rejected paths, which is exactly what keeps a later worker from walking into the same dead end again. Filling the publication with the traffic of the genesis makes it unreadable for consumers and gives away internals nobody has checked.

**Obvious and rejected.** *One repository, the genesis in a subdirectory, everything public*: gives away internals and makes the history unreadable; and once published, there is no way back. *One repository, the history smoothed before publication*: destroys the genesis for good; whoever does this notices the loss only when somebody asks "why, actually?". *Two branches instead of two repositories*: branches share access rights; you cannot publish one and hold back the other. *The genesis outside version control, in a wiki, say*: then it ages separately and is not at hand when the work continues.

**How you notice it is violated.** History is rewritten before a publication, or somebody hesitates to record something because it might become visible later.
