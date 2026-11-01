---
type: Proposal Rule
description: The ability sits in the token itself; the service derives everything that is allowed from the token, with no caller-chosen scope parameter and no rights ordered in the issuing call.
number: F3
binding: must
role: load-bearing
applies_to: [operator]
---

# F3 · The token is the scope

**Applies to:** Operator.

**What holds.** The ability sits in the token itself, not in a caller-selectable parameter of the call: the service checks the token and derives from it, completely, what is allowed. There is no scope parameter that the caller sets, and no rights that are ordered along with the issuing call.

**Why.** A selectable parameter is authorization code that can be wrong — and an agent that takes things literally sets it literally wrong. An ability that lives in the artifact is not negotiable: a token that ends up in the wrong place *cannot* do more there than it embodies.

**Obvious and rejected.** *A scope parameter on the call, checked against the token* — redundancy with room for error: two places have to agree, and one of them is set by the attacker. *Ordering rights in the same call that creates the token* — then the anonymous party decides on their own cut; the separation of creation and activation is the protection.

**How you notice it is violated.** A call with the same token has a different reach depending on a parameter sent along with it.
