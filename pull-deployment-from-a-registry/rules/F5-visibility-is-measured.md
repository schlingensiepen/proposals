---
type: Proposal Rule
description: Whether a published image reaches the operator is decided by a measurement, fetching the manifest under the name the deployment file uses with exactly the credentials the operator will have, for a public image with none, not by a setting.
number: F5
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F5 · Visibility is measured: the anonymous manifest fetch

**Applies to:** builder · operator.

**What holds.** Whether a published image reaches the operator is decided not by a setting but by a **measurement**: fetching the manifest under the name the deployment file uses, with exactly the credentials the operator will have — for a public image, therefore, **without any login**, from a machine or a step that holds no stored credentials for the registry. The measurement is repeated after every change to the visibility, is part of the release instructions, and wherever the build pipeline can do it, it belongs there — as a step of its own without the build credential, after publishing. The deployment file says whether a login is needed and, if not, how the operator knows (F6).

**Why.** The visibility of the source repository and that of the image are two settings, often in two places, and there is a third: whether the organization under which the image is published allows public images at all. A private image answers an anonymous fetch with a message that looks like a login problem ("unauthorized"), and the operator searches their credentials instead of a setting — fault-finding in the wrong direction whose cause lies with the builder. Looking at the settings checks an intention; the anonymous fetch checks the result, and only the result reaches the operator. Conversely, the same measurement protects against an image being public that should not be.

**Obvious and rejected.** *Inferring it from the repository setting* — the incident. *Looking it up in the registry's user interface* — shows the builder's intention, not the organization's rule and not the behavior toward an anonymous caller. *Pulling with your own machine* — the builder is logged in everywhere; their success proves nothing for the operator. *Having the operator log in when in doubt* — hands out credentials for a problem that is not a credential problem, and leaves the wrong setting in place.

**How you notice it is violated.** An operator reports "unauthorized", and their credentials are fine — or an image can be fetched from outside, and nobody decided that.
