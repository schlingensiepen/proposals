---
type: Proposal Sources
description: What this proposal builds on; the imported proposal, related proposals of the series and external sources.
sources:
  - id: adr
    title: Architecture Decision Records (Michael Nygard, 2011) and the practice that grew from them
  - id: agent-entry-files
    title: Widespread conventions for agent entry files in the root directory of a repository
---

# Sources

## Imports

* `proven-from-outside` (imports): a document meant to work without inside knowledge counts as accepted only once a reader without that knowledge has completed a task with it. The dead-link test of F1 takes this over: it is applied by a reader from outside, not by the writer. Its reasons are not repeated here.

## Related

* `two-repo-submodule-pattern` (overlaps-with): applies the inlining rule of this concept (F5) to the passage of content between two repositories, copying instead of linking. Each is a deepening of the other, never a prerequisite; each must remain understandable without the other.
* `self-describing-services` (overlaps-with): the same idea on the level of running services, one source instead of scattered copies, where this proposal applies it to the repository. A deepening, not a prerequisite for understanding.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

The locations below are named as in the drafting work; they have not yet been checked against a retrieval date.

* Architecture Decision Records (Michael Nygard, 2011) [^adr]: the widespread form of recording decisions with context, decision and consequences. The starting point of F5, and at the same time the place where this concept is stricter: the widespread practice *links* to the decision record; F5 brings statement and reasons into the document that needs them.
* Conventions for agent entry files [^agent-entry-files]: the place where the entry point of F2 is actually looked for; how this concept relates to them is not yet settled (see the scope).

[^adr]: Architecture Decision Records, introduced by Michael Nygard in 2011, and the practice that grew from them.
[^agent-entry-files]: Widespread conventions for agent entry files in the root directory of a repository.
