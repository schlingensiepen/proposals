---
type: Proposal Sources
description: What this concept builds on; no imports, one proposal recommended alongside it, no external standards, and the reference implementation.
sources:
  - id: imap-service
    url: https://github.com/schlingensiepen/imap-service
    title: The author's mail-filing service, repository imap-service (not publicly accessible)
---

# Sources

## Imports

This concept imports no other proposal.

## Related

* `service-as-single-artifact` (recommended-with): a service built that way has its commands as one surface and its operations as callable functions; anyone who places a second surface beside the commands applies this concept so that the second surface does not become a second implementation.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

This proposal cites no external standards, because none is load-bearing for its rules.

## Reference implementation

* The author's mail-filing service, repository `imap-service` [^imap-service]: the reference implementation in the evidence (one operation per administrative action, two pass-throughs, structural tests on both sides, the incident). The repository is not publicly accessible.

[^imap-service]: https://github.com/schlingensiepen/imap-service (not publicly accessible)
