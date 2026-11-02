---
type: Proposal Rule
description: The running service offers no administration endpoints, debug routes or management UI; diagnosis, inspection and change are commands, authenticated by server access.
number: F7
binding: must
role: load-bearing
applies_to: [builder]
---

# F7 · The running service has no management surface

**Applies to:** builder.

**What holds.** The service mode offers no administration endpoints, no debug routes and no management UI. Checking, inspection and change are commands. These need **no login of their own**: they are started locally by someone who already has server access — this access *is* the authentication. This does not cover pure operational endpoints without effect and without information about contents, a readiness point, say, that says nothing but "running"; where their boundary runs is not yet settled (see the scope).

**Why.** A management surface on the network is reachable around the clock, whereas a command runs only when someone starts it. It needs a login of its own, a permission check of its own and error handling of its own — three new places where something can be wrong, for a task that presupposes server access anyway. And it is the most convenient path for an attacker who already has the network surface.

**Considered and rejected.** *A management path behind a simple login* — the weakest login of the system guards its strongest effect. *A debug endpoint "internal only"* — "internal" is a network assumption, not a property. *Runtime switching of behavior over the network surface* — makes the service configurable from outside and the configuration file a fiction (F5).

**How you notice it is violated.** In the reverse-proxy configuration a path has to be blocked explicitly — or a block is missing, and nobody notices.
