---
type: Proposal Sources
description: What this proposal builds on; the concepts it imports, related proposals of the series, and external sources.
sources:
  - id: twelve-factor-admin
    url: https://12factor.net/admin-processes
    title: The Twelve-Factor App, XII. Admin processes
  - id: twelve-factor-config
    url: https://12factor.net/config
    title: The Twelve-Factor App, III. Config
---

# Sources

## Imports

* `one-configuration-one-judgment` (at F5): how the configuration is found, judged and reported, with its choices on precedence and the empty value.
* `where-a-secret-comes-from` (at F5 and F9): where a start secret comes from, in which ranking, and what a ranking obliges.
* `maildir-style-file-queues` (at F12): the mechanics for a file that more than one process writes.

## Related

* `capability-tokens-for-agents`: the justification of the access pattern that F9 presupposes, one token, one capability, one place.
* `self-describing-services`: how the finished service describes itself to its consumers; a separate matter from how it is built.
* `invitation-instead-of-login`: how an access is issued to a person for all their devices; this concept issues it to a configuration.
* `service-as-a-container`: what changes when the artifact runs as a container.
* `push-deployment-that-keeps-its-secrets` and `pull-deployment-from-a-registry`: the two ways the one deliverable of F4 reaches its target.

The running example in the evidence is the author's draft-filing service; its repository is not publicly accessible. All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* The Twelve-Factor App, XII. Admin processes [^twelve-factor-admin]: admin processes run in an identical environment to the regular processes and ship with the application code. This concept takes it one step further: admin code *is* the application code, in another mode.
* The Twelve-Factor App, III. Config [^twelve-factor-config]: strict separation of configuration from code; adopted here as the separation of configuration from the artifact, extended by the separation of configuration from state (F12).

[^twelve-factor-admin]: https://12factor.net/admin-processes
[^twelve-factor-config]: https://12factor.net/config
