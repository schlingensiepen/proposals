---
type: Proposal Sources
description: What this proposal builds on; related proposals, one tension, and external sources.
sources:
  - id: twelve-factor-config
    url: https://12factor.net/config
    title: The Twelve-Factor App, III. Config
---

# Sources

## Related

* `one-configuration-one-judgment` (sister concept): how the
  configuration is judged and the source shown per value; it imports
  this concept for the reference to secrets.
* `capability-tokens-for-agents`: what a secret may do.
* `invitation-instead-of-login`: how a secret is issued.
* `push-deployment-that-keeps-its-secrets`: the script places the
  secrets in through rank 1 or 2 of this ranking.

## Tension

* `get-only-exit` carries the secret in the address; that is a use, not
  a source, and it explicitly suspends
  [F6](./rules/F6-never-in-any-output.md) there.

All proposals live in the same repository; see its root README for the
register and the links. Which proposals build on this one is listed in
[`used-by.md`](./used-by.md).

## External sources

* The Twelve-Factor App, III. Config [^twelve-factor-config]: secrets in
  the environment. This concept adopts the environment as rank 1 and
  adds what is missing: the file as a pass-through, the once-only
  channel, and the duties of a ranking.

[^twelve-factor-config]: https://12factor.net/config
