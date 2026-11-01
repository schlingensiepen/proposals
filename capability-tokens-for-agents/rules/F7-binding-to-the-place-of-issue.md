---
type: Proposal Rule
description: A token is used only against the place it was issued for, one token per instance or target system; the agent asks on ambiguity, and the operator checks the binding server-side where the topology allows.
number: F7
binding: must
role: load-bearing
applies_to: [agent, operator]
---

# F7 · Binding to the place of issue

**Applies to:** Agent (delivered by the operator, F12) · Operator.

**What holds.** A token is used exclusively against the place it was issued for. Per instance or target system there is a token of its own; on ambiguity ("put it in the wiki" — but which one?), the agent asks instead of guessing. The operator SHOULD additionally check the binding server-side wherever the topology allows it (host or tenant binding): a token that has ended up in the wrong place then runs into nothing.

**Why.** Same software does not mean same trust: the company system and the private one share the API and nothing else. The mix-up reports no error — the call mechanics fit perfectly, only the target is wrong. The double binding (behavioral rule at the agent, check at the operator) dampens exactly the case in which third parties abuse the agent as a confused deputy.

**Obvious and rejected.** *One token per software with a target list inside it* — merely shifts the danger of mix-up inward. *Guessing the target from the context* — an agent that takes things literally also guesses literally wrong.

**How you notice it is violated.** A contribution lands in the wrong system — or a secret is used against a target other than the one it was issued for.
