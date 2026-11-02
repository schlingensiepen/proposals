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

**What holds.** Self-description has a state between "does not exist" (404) and "here is the description": "exists, but is not set up yet", the **empty** state. The service names it and decides what its entry page answers in this state: a setup page with status 200, or 404, with the price of each direction. The decision is recorded together with its price. A setup page is part of the one source (F1) and is delivered in the same form as the entry page (F16).

**Why.** If nobody decides this, it is decided by accident, and usually in favor of 404, because that is what results from the configuration logic. Then the description is missing at the one moment somebody needs it in order to set the service up. If you promise that a service can be opened up from its URL, you promise it for the first call as well.

**Considered and rejected.** *Binding the cascade to the configuration*: as long as nothing is set up, the entry page answers 404, not because anybody decided so but because it results from the configuration logic. That is how the first version of the reference implementation behaved; it reliably shut out every agent and was the exact opposite of the purpose.

**How you notice it is violated.** The person who is to set up the service finds nothing at its address that tells them what they have in front of them, and nobody can say whether the 404 before setup was decided or merely happened.
