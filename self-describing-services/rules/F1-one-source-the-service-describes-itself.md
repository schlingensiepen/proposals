---
type: Proposal Rule
description: The authoritative description of a service, entry page, usage description and interface description, is delivered by the service itself at runtime; there is no separately distributed copy.
number: F1
binding: must
role: load-bearing
applies_to: [builder, façade operator]
---

# F1 · One source: the service describes itself

**Applies to:** builder (in a retrofit: façade operator).

**What holds.** The authoritative description of a service — entry page, usage description, interface description — is delivered by the service itself at runtime; a separately distributed and separately maintained description does not exist.

**Why.** Every copy begins to go stale the moment it is created, and agents believe instructions literally: an outdated description gives rise to perfectly formatted wrong calls. At the same time, with distribution the installation disappears too — and with it the N×M problem of services and platforms.

**Obvious and rejected.** *Distributing skill files* — they go stale and have to be brought to every consumer; precisely the original problem. *A connector or broker service per platform* — a second implementation of the API that has to be installed and that drifts on its own. *Documentation discipline and release checklists* — they fight the symptom; structurally the second source remains.

**How you notice it is violated.** Two descriptions of the same service contradict each other in a detail. The early indicator is small: an operation count that reads "12" in one artifact and "13" in the neighboring document — a case actually observed, resolved in the evidence.
