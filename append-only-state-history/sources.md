---
type: Proposal Sources
description: What this proposal builds on; it imports nothing, names one related proposal of the series and one related external approach, whose references are not yet checked.
sources: []
---

# Sources

## Imports

This proposal imports no other proposal.

## Related

* `immutable-decision-records` (overlaps-with): the same idea, replacing instead of changing, on another subject; a deepening, not a prerequisite. This concept differs in that attachments are carried over by reference instead of being replaced along with everything else; the reason is F2.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

*Fragment: the references are not yet checked and carry no retrieval date in version 0.3.*

* **Event sourcing**, the related and considerably heavier approach: it stores changes instead of states and computes the current state from them. The distinction is still to be worked out: this concept stores **states**, not events, and therefore needs no replay.
