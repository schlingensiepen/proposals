---
type: Proposal Sources
description: What this proposal builds on; no imports, the mechanism it gives a form to, and the external sources on MADR and its origin.
sources:
  - id: madr
    url: https://adr.github.io/madr/
    title: MADR, Markdown Any Decision Records, version 4.0.0
  - id: nygard-2011
    url: https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions
    title: Michael Nygard, Documenting Architecture Decisions (2011)
  - id: adr-collection
    url: https://adr.github.io/
    title: Collection of templates and field reports on decision documentation
---

# Sources

## Imports

None. Everything this concept needs is written out in this bundle.

## Related

* `immutable-decision-records` (form-of): this concept is the form; that proposal is the mechanism the records here are subjected to. Deepening, not a prerequisite, and the origin of the rules that the concept spells out on immutability. **This dossier is fully usable without it**: the rules it rests on are spelled out here, with their reasons.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* MADR, Markdown Any Decision Records, version 4.0.0, with templates and field descriptions [^madr] (retrieved 2026-08-13).
* Michael Nygard, "Documenting Architecture Decisions" (2011), the original format without a YAML head from which MADR grew [^nygard-2011] (retrieved 2026-08-13).
* Collection of templates and field reports on decision documentation [^adr-collection] (retrieved 2026-08-13).

[^madr]: https://adr.github.io/madr/
[^nygard-2011]: https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions
[^adr-collection]: https://adr.github.io/
