---
type: Proposal Rule
description: Without an operator setting there is no administration surface; an empty address means no port, no secret, no generation; the delivery form addressed to the operator without a shell switches it on through its environment, overridably.
number: F5
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F5 · Switched off as shipped: the delivery form switches it on

**Applies to:** builder · operator.

**What holds.** Without a setting by the operator there is no administration surface: an empty operational parameter for its address means **no port, no administration secret, no generation**, even when the environment variable for the secret is set. It is switched on by the **delivery form whose addressee is the operator without a shell**: the container image sets the address in its environment, so that a deployment file can override it and switch the surface off again; the package for running on the host sets nothing. The deployment file that switches the surface on says in its head that it does so and what that costs.

**Why.** An administration surface that is there by itself is an attack surface nobody ordered; and a service that generates and stores an administration secret it never serves has enlarged its attack surface for nothing. The as-shipped state is the base cut: no surface, no root. The image switches it on because the image is built for exactly the operator for whom the surface exists, and because an image that leaves the administration off could not be put into operation by him; the same reason for which there is a surface at all. As environment and not as an invocation argument, because an argument would override the operator: he could no longer switch the surface off without changing the image.

**Obvious and rejected.** *The surface always on, "it is authenticated after all"*: a cut that gets by without it would get it everywhere, including where there is a shell. *Generating the secret even when the surface is off, "so it is ready"*: a root secret on disk that serves nothing, for a case that may never occur. *The address as a fixed invocation argument in the image's entry point*: then it wins against every deployment file, and the surface can no longer be switched off in the container.

**How you notice it is violated.** A service on the host listens on an administration port nobody configured; or a data directory holds an administration secret although the surface is off; or an operator cannot switch the surface off in the container without rebuilding the image.
