---
type: Proposal Rule
description: The work repository embeds the product repository, never the reverse; the product repository knows nothing of the workshop and contains no reference to it.
number: F2
binding: must
role: load-bearing
applies_to: [author]
---

# F2 · The work embeds the product, never the reverse

**Applies to:** author.

**What holds.** The work repository embeds the product repository. The product repository knows nothing of the workshop and contains no reference to it.

**Why.** Whoever works needs both at once; whoever only uses needs only one. Embedding in this direction serves both without either side dragging anything along. The other way round, the reference would be useless or worse: a public product that points at a non-public repository leads every reader into a dead end, and reveals that repository's existence in passing.

**Obvious and rejected.** *Mutual references*: create a circular relation and the question which state fits which. *A reference from the product to the workshop "for those interested"*: those interested get an error message; whoever wants to show the genesis publishes it and thereby treats it as product. *Both side by side as equals, without embedding*: works, but loses the fact that they belong together; you then have to know by hand which directories are checked out together.

**How you notice it is violated.** The published product contains a reference that nobody outside can follow.
