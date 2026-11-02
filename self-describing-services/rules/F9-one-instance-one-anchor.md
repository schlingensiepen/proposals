---
type: Proposal Rule
description: Remembering, anchoring and authorizing happen per instance, never per software; when a request is ambiguous, the agent asks.
number: F9
binding: must
role: independent
applies_to: [consumer]
---

# F9 · One instance, one anchor

**Applies to:** consumer (delivered by the builder, F11).

**What holds.** Remembering, anchoring, and authorizing happen per **instance**, never "per software": every instance of the same service gets its own anchor with its own purpose, its own base URL, and its own secret. If the user has anchored several instances of the same service, the agent asks back when things are ambiguous ("put that in the wiki" — but which one?) instead of guessing.

**Why.** Instances share the API, but nothing else: different data, different rights, different secrets. An agent that conflates them writes into the wrong wiki or uses one instance's secret against the other — with the same, perfectly fitting call mechanics that will report no error.

**Considered and rejected.** *One anchor per piece of service software with a list of instances inside it* — merely shifts the risk of confusion inward and lets the anchor grow into the registry that this concept deliberately does not build (F10). *Guessing the instance from context* — the agent who takes things literally also guesses literally wrong.

**How you notice it is violated.** A contribution lands in the wrong instance — or a secret is used against a base URL other than the one it belongs to.
