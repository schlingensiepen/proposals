---
type: Proposal Rule
description: The administration surface is a separate application on its own port under its own published name, no path prefix; the two surfaces share no login, the administration is absent from the user-facing description, and an unbindable port aborts the start.
number: F4
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F4 · Own port, own application, own name: no path prefix

**Applies to:** builder · operator.

**What holds.** The administration surface is a **separate application on its own port**, with its own path namespace (in the pattern: `/admin/v1`) and its own self-description (F11), and the reverse proxy in front publishes it under an **own name**, ideally in another zone, which it keeps as tight as the environment allows. The user-facing surface stays unchanged: no administration path, no debug path, a readiness point that says only "running"; metrics exist only behind the administration secret on the other port. The two surfaces share **no login**: an access to the function is not accepted on the administration surface, the administration secret not on the user-facing surface, and this structurally, because the administration secret is not in the store and an access is never compared with it, not by a rule somebody has to remember; and the refusal is the same for "no value", "wrong scheme" and "wrong value". If the user-facing surface has a description anybody can read, the administration surface does **not** appear in it; a consumer whom the administration does not concern does not even see it. A port that cannot be bound aborts the start.

**Why.** A path prefix on the same entry point makes protection a matter of proxy configuration: a forgotten or wrongly ordered rule puts the administration on the network without anybody noticing. What is not routed to an address of its own, by contrast, is not reachable there; the separation is structural instead of checked. The own name gives the operator the lever to keep the administration tighter without touching use: a source address list, an access network, a second factor at the proxy, everything that would be wrong for the user-facing surface. The separate login without a rule, because an attacker does not read the rule and a builder forgets it at the third endpoint. And the invisibility in the user-facing self-description, because that is open and anybody reads what is in it; the administration surface does not belong on the map a stranger gets.

**Considered and rejected.** *Administration routes in the same namespace*: saves a port and moves security into a configuration line. *A shared login with roles ("this token may administer")*: introduces the permission model the base cut saves, and turns the user-facing surface back into an administration path. *Linking the administration from the entry of the user-facing surface "so one finds it"*: anyone who needs it knows it exists; anyone who does not need it should not find it. *Leaving the administration port in the container unbound if it is taken*: then the service runs without administration and does not say so.

**How you notice it is violated.** A path has to be blocked explicitly in the proxy configuration; or an access to the function opens an administrative action; or the open self-description of the user-facing surface names an administration route.
