---
type: Proposal Rule
description: Every document stays understandable and followable when every link to the outside is dead; outside knowledge is brought in with statement and reasons; links are evidence or deepening, never a prerequisite; redundancy is allowed.
number: F1
binding: must
role: load-bearing
applies_to: [writer, maintainer]
---

# F1 · The check rule: does it work with dead links?

**Applies to:** everyone who writes in the repository.

**What holds.** Every document in the repository must remain understandable and followable when every link to the outside points into nothing. Outside knowledge is brought in with its statement and reasons; links are evidence or deepening, never a prerequisite. Redundancy is expressly allowed for this.

**Why.** The rule turns an intention into a check question that can actually be asked during review, and that reliably fires. It also hits exactly the difference between human and agent: the human follows a dead link and asks if need be; the agent does neither, but fills the gap with invention. And it protects against a deception that otherwise goes unnoticed: a document with dead links looks complete.

**Considered and rejected.** *Trusting reliable links*: links to internal systems do not survive moves, renamings and changes of tool; their half-life is shorter than that of the repository. *Avoiding redundancy ("a single source of truth")*: right for data, wrong for explanations; an explanation that is in only one place is absent from all other places. The price is that redundancy must be maintained (F6). *A link checker in the build*: finds dead links, but not the actual problem: that a living link is a prerequisite for understanding.

**How you notice it is violated.** A reader has to follow a link in order to keep working; or somebody answers a question about the repository from memory instead of from the repository.
