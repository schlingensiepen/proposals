---
type: Proposal Sources
description: What this proposal builds on; it imports nothing, names the proposal that gives it a concrete form, and lists the external sources.
sources:
  - id: madr
    url: https://adr.github.io/madr/
    title: MADR, Markdown Any Decision Records, version 4.0.0
  - id: nygard-adr
    url: https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions
    title: 'Michael Nygard, "Documenting Architecture Decisions" (2011)'
  - id: adr-overview
    url: https://adr.github.io/
    title: Architectural Decision Records, overview of templates and field reports
---

# Sources

## Imports

This proposal imports no other.

## Related

* `decision-records-with-madr` (form-of, from that side): the form of this mechanism. Whoever applies the mechanism to a body of architecture or software decisions finds a ready form there. This proposal is fully usable without it: the mechanism is deliberately described independently of any format and holds for any decision log.
* `self-describing-services` (recommended-with): named by that proposal from its side; see there.
* `append-only-state-history` (overlaps-with): named by that proposal from its side; see there.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* MADR, Markdown Any Decision Records, a format standard for decision records, version 4.0.0 [^madr] (retrieved 2026-08-13). This concept uses MADR as its file format and extends it with the two relation fields as lists (F5).
* Michael Nygard, "Documenting Architecture Decisions" (2011) [^nygard-adr], the original description of the method including the principle of not changing accepted records but replacing them (retrieved 2026-08-13).
* Overview of decision documentation, a collection of templates and field reports [^adr-overview] (retrieved 2026-08-13).

[^madr]: https://adr.github.io/madr/
[^nygard-adr]: https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions
[^adr-overview]: https://adr.github.io/
