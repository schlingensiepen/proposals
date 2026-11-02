---
type: Proposal Sources
description: What this extension builds on; the concept it extends, the concepts it imports, related proposals of the series, and the reference implementation.
sources:
  - id: imap-service
    url: https://github.com/schlingensiepen/imap-service
    title: The author's mail-filing service, repository imap-service (not publicly accessible)
---

# Sources

## Extends

* `service-as-single-artifact`: the base cut this extension presupposes. This extension is the HTTP form of that concept's choice W1 (b), commands brought into the system over another channel; it lifts F7, F10 and the part of F8 under which the service mode does not write the store. Everything needed here is written out in this bundle.

## Imports

* `where-a-secret-comes-from` (at F6, F7, F8): the ranking of sources for a start secret, the report of lower ranks, replacing in two moves, and the one named exception to "never in any output". This extension decides that concept's choices for the administration secret: generate and show once when none is set, abort when it is set but unusable.
* `one-operation-many-surfaces` (at F2, F3): every operation exists exactly once, and every surface is a pass-through onto it, checked structurally. F2 and F3 say only what this extension adds for the command line and the administration surface.
* `proven-from-outside` (at F11 and step 12 of the procedure): a document meant to work without inside knowledge is accepted only by a reader who has none. The entry page and the refusals of this surface are accepted that way; this extension does not repeat the test, it imports it.

## Related

* `get-only-exit` (overlaps-with): another second surface born of need; the word "pass-through" is shared.
* `capability-tokens-for-agents` (recommended-with): the form of the accesses this surface issues. The administration secret is not a capability token but the root from which such tokens are issued (F9).
* `self-describing-services` (recommended-with): a possible complement; this extension orders only the core of F11.
* `push-deployment-that-keeps-its-secrets` (alternative-to): a delimitation, not evidence: the opposite direction. This extension treats the case without a channel inward. The line runs not between pushing and pulling but between "there is a way inward" and "there is none".
* `pull-deployment-from-a-registry` (recommended-with): delivery without a channel inward as a whole; this extension answers only how the first secret comes into being.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

This proposal cites no external standards, because none is load-bearing for its rules.

## Reference implementation

* The author's mail-filing service, repository `imap-service` [^imap-service]: the reference implementation in the evidence (administration surface, incidents, state of play). The repository is not publicly accessible.

[^imap-service]: https://github.com/schlingensiepen/imap-service (not publicly accessible)
