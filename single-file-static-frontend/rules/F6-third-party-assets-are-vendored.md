---
type: Proposal Rule
description: Third-party libraries are versioned files in the artifact; no script and no stylesheet is loaded from an external server.
number: F6
binding: must
role: independent
applies_to: [builder]
---

# F6 · Third-party assets are vendored, not linked

**Applies to:** builder.

**What holds.** Third-party libraries are files in the artifact, versioned. No script and no stylesheet is loaded from an external server.

**Why.** A linked third-party script runs **in your own origin** and can do everything your own code can. Anyone who serves it thereby decides the content of your application at any time — without anyone noticing. This is not a question of trusting a particular address but of how many parties have a say in what gets delivered; the right answer is one. On top of that: a vendored asset also works when the external server is unreachable, and it still works in ten years.

**Considered and rejected.** *Link with an integrity check* — rules out substitution but not outage, and ties your availability to someone else's. *Link, "it is the common address after all"* — popularity is not protection but an incentive for whoever takes it over.

**How you notice it is violated.** The application is broken in airplane mode or behind a restrictive firewall.
