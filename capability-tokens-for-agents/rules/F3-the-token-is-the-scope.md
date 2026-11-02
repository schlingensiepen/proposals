---
type: Proposal Rule
description: The ability is in the token itself; the service derives everything that is allowed from the token, with no caller-chosen scope parameter and no rights requested in the issuing call; a token created without rights and activated later is no exception.
number: F3
binding: must
role: load-bearing
applies_to: [operator]
---

# F3 · The token is the scope

**Applies to:** Operator.

**What holds.** The ability is in the token itself, not in a caller-selectable parameter of the call: the service checks the token and derives from it, completely, what is allowed. There is no scope parameter that the caller sets, and no rights that are requested along with the issuing call. A token created without rights and activated later (F5) is no exception: the rights the user grants are attached to the token on the service side, and the service still derives everything from the token alone.

**Why.** A selectable parameter is authorization code that can be wrong — and an agent that takes things literally sets it literally wrong. An ability that lives in the artifact is not negotiable: a token that ends up in the wrong place *cannot* do more there than it embodies.

**Considered and rejected.** *A scope parameter on the call, checked against the token* — redundancy with room for error: two places have to agree, and one of them is set by the attacker. *Requesting rights in the same call that creates the token* — then the anonymous party decides on their own cut; the separation of creation and activation is the protection.

**How you notice it is violated.** A call with the same token has a different reach depending on a parameter sent along with it.
