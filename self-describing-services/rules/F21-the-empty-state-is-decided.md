---
type: Proposal Rule
description: Self-description has an empty state between "does not exist" and "here is the description"; the service names it, decides whether its entry page answers there with a setup page and 200 or with 404, and records the decision with its price.
number: F21
binding: must
role: load-bearing
applies_to: [builder]
---

# F21 · The empty state is decided

**Applies to:** builder.

**What holds.** Self-description has a state between "does not exist" (404) and "here is the description": "exists, but is not set up yet", the **empty** state. The service names it and decides what its entry page answers in this state: a setup page with status 200, or 404, with the price of each direction. The decision is recorded together with its price.

**Why.** Whoever does not decide this decides it by accident, and usually in favor of 404, because that is what falls out of the configuration logic. Then the description is missing at the one moment somebody needs it in order to set the service up. Whoever promises that a service can be opened up from its URL promises it for the first call as well.

**Obvious and rejected.** *Binding the cascade to the configuration*: as long as nothing is set up, the entry page answers 404, not because anybody decided so but because it falls out of the configuration logic. That is how the first version of the reference implementation behaved; it reliably shut out every agent and was the exact opposite of the purpose.

**How you notice it is violated.** Whoever is to set up the service finds nothing at its address that tells them what they have in front of them, and nobody can say whether the 404 before setup was decided or merely fell out.
