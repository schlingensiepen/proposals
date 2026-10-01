---
type: Proposal Sources
description: What this proposal builds on; the concept it imports, related proposals, and external sources.
sources:
  - id: twelve-factor-config
    url: https://12factor.net/config
    title: The Twelve-Factor App, III. Config
---

# Sources

## Imports

* `where-a-secret-comes-from` (sister concept, imported by
  [F3](./rules/F3-secrets-are-referenced-not-contained.md)): where a
  start secret comes from and in which ranking.

## Related

* `self-describing-services`: a service's self-description tells the
  consumer what the service needs; this concept tells the operator what
  the service read. Same way of thinking, no overlap.

All proposals live in the same repository; see its root README for the
register and the links. Which proposals build on this one is listed in
[`used-by.md`](./used-by.md).

## External sources

* The Twelve-Factor App, III. Config [^twelve-factor-config]:
  configuration in the environment. This concept adopts the separation
  of code and configuration but rejects "everything through environment
  variables" as the only form.

[^twelve-factor-config]: https://12factor.net/config
