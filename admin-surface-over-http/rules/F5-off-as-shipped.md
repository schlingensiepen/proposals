---
type: Proposal Rule
description: Without an operator setting there is no administration surface; an empty address means no port, no secret, no generation; the image delivers the surface but does not switch it on, and whether it is switched on is decided with the user and recorded, set as environment of the deployment so that it can be switched off again.
number: F5
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F5 · Switched off as shipped: switching it on is a decision, not a default

**Applies to:** builder · operator.

**What holds.** Without a setting by the operator there is no administration surface: an empty operational parameter for its address means **no port, no administration secret, no generation**, even when the environment variable for the secret is set. The container image **delivers** the surface, it does not switch it on: the image sets no address, and neither does the package for running on the host. Whether the surface is switched on is a **decision the adopting agent puts to the user** who runs the service, with the reason recorded; the operator then sets the address in the environment of his deployment, as environment and not as an argument baked into the image, so that a deployment file can override it and switch the surface off again. The deployment file that switches the surface on says in its head that it does so, why, and what that costs.

**Why.** An administration surface that is there by itself is an attack surface nobody ordered; and a service that generates and stores an administration secret it never serves has enlarged its attack surface for nothing. The as-shipped state is the base cut: no surface, no root. The image does not switch it on, because an image is pulled by every operator, with a shell and without; an image that switches on makes the departure of F1 for all of them, and nobody decided it. The one who decides is the one who runs the service and knows whether he has a shell; that is why it is a question to the user and not a default of the delivery form. As environment and not as an invocation argument, because an argument would override the operator: he could no longer switch the surface off without changing the image.

**Considered and rejected.** *The surface always on, "it is authenticated after all"*: a cut that gets by without it would get it everywhere, including where there is a shell. *The image switches the surface on for everyone who pulls it*: the departure of F1 made for every operator, with a shell or without, by the delivery form instead of by a decision. *Generating the secret even when the surface is off, "so it is ready"*: a root secret on disk that serves nothing, for a case that may never occur. *The address as a fixed invocation argument in the image's entry point*: then it wins against every deployment file, and the surface can no longer be switched off in the container.

**How you notice it is violated.** A service listens on an administration port nobody configured; or a data directory holds an administration secret although the surface is off; or the surface is on and nobody can say who decided that and why; or an operator cannot switch the surface off in the container without rebuilding the image.
