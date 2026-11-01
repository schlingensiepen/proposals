---
type: Proposal Rule
description: Exactly one delivery artifact is built and published; commands run against it in the same environment; the deployment file names the published artifact instead of building locally; the deliverable says which revision it was built from.
number: F4
binding: must
role: load-bearing
applies_to: [builder, operator]
imports: [build-stamp]
---

# F4 · One delivery artifact, one delivery

**Applies to:** builder · operator. Imports `build-stamp`.

**What holds.** Exactly one delivery artifact is built and published. Commands run against this delivery artifact in the same environment as the service (for example: on the host the same binary; in container operation an invocation against the same image, the rules for which belong to a container concept of its own, `service-as-a-container`, not here). There is no second, "slim" administration image. Part of the delivery artifact is **how it comes into being and how it arrives**: a build pipeline that produces and publishes it reproducibly from the source, and a deployment file that **names** exactly this published delivery artifact instead of rebuilding it on the target machine. If it is built locally after all — in development, or because the target machine cannot reach a registry — that is a named exception and is marked as such in the delivery directory. The deliverable says which revision it was built from, and the build pipeline publishes nothing that cannot; the rule and its reasons are in the imported concept `build-stamp`.

**Why.** A second delivery artifact can have a different version, different libraries, different paths, a different time zone — and then the command sees something other than the service does. The identical environment is not a convenience but the precondition for a check result saying anything about the service at all. The same argument carries all the way into the deployment file: whoever has it build locally ends up with as many delivery artifacts as machines — each with the state of the base layers from the day it was built. "One delivery artifact" is then no more than an intention.

**Obvious and rejected.** *An administration image of its own without the service's dependencies* — saves megabytes and buys them with version drift. *The deployment file builds locally* — convenient the first time, after that the multiplication of the artifact: two machines with the same version number can behave differently, and nobody sees why. Fine as a development convenience, not as a delivery path. *Administration as a sidecar container* — the same separation, only with more moving parts. *Commands from the developer's machine against the production store* — a different environment, a different version, and the only protection is attentiveness.

**How you notice it is violated.** Someone has to ask which version the administration tool has — or a command behaves differently on the server than locally — or two machines carry the same version number and behave differently, because each built its image itself.
