---
type: Proposal Sources
description: What this proposal builds on; the imported and the related proposal of the series and the external sources behind the submodule mechanics, the tool limit and the review and release rules.
sources: []
---

# Sources

## Imports

* `proven-from-outside` (imports): a document meant to work without inside knowledge counts as accepted only once a reader without that knowledge has completed a task with it. The review before every handover (F7) takes this over as its comprehension check for the self-contained documents beyond the handover, and does not repeat its reasons.

## Related

* `self-contained-repos` (overlaps-with): the rule that knowledge is carried over with its statement and its reason instead of being linked is argued there in detail; F3 applies it to the handover between two repositories. Further reading, not needed for understanding this concept, which stands on its own.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* Documentation on submodules in the widely used version control system: the mechanics of F4, and the explanation of why a submodule reference is an entry type of its own, the fact on which the tool limit in F5 depends.
* The interface documentation of the hosting provider used, on file operations: the evidence for the limit in F5; files can be created and changed, submodule references cannot.
* The author's draft-filing service with its work repository: the application described in the evidence. Neither repository is publicly accessible.
* A note from a second project that applies this pattern, answering the open points of this draft: the origin of the regeneration sentence in F3, of the review in F7, of the release in F8 and of the usual allocation in the concept. Not publicly accessible.
