---
type: Proposal Rule
description: The administration secret is a start secret under the imported where-a-secret-comes-from; what this extension adds is that it is a root and that, when no rank holds it, the service generates it and writes it into its log exactly once.
number: F6
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F6 · The service generates the administration secret and names it exactly once in its log

**Applies to:** builder · operator.

**What holds.** When the surface is switched on, its secret is a start secret in the sense of `where-a-secret-comes-from`, and that concept applies to it unchanged: its ranking (an environment variable, in the pattern `<SERVICE>_ADMIN_TOKEN`, then a pass-through file beside the store, last the once-only channel), its report of lower ranks that are also set, replacing in two moves, and its one named exception to "never in any output". **What this extension adds:** the administration secret is a **root** (F9). For it, that concept's choice "no secret set" is decided as "generate and show exactly once": the service generates it, stores it and writes it into its log **exactly once**, one value, once, only at generation; never for a secret somebody set, never at restart, never for an access to the function. An agreed value in the variable (in the pattern `new`) forces regeneration; "exactly once" then means once **per generation**: the new secret appears once, the old one never again. The build pipeline starts the deliverable before publishing it and requires that the secret appears in the log exactly once per generation, is accepted, that one not issued is refused, and that it still applies after a restart without appearing again.

**Why.** When an image is pulled, nobody but the service itself is inside; so only it can generate the first secret, and the log is its only channel back to the operator. The imported concept justifies the ranking and the exception to the rule against secrets in logs; what is said here is only why generation is chosen for this root: without it the operator without a shell would have no way in (the gap), or would find one (the workaround): the secret in the image, in the deployment file, in a ticket. Anyone who can set the variable from a platform's secret management does not need the exception and does not trigger it.

**Considered and rejected.** *Letting the operator enter a secret*: the weaker secret, and taking the service into operation begins with somebody making up a value. *Shipping a first secret with the image*: it is in every copy of the image. *Showing it again after loss*: loss means replacement (F9).

**How you notice it is violated.** An administration secret appears more than once per generation in the log; or one the operator set appears there at all; or taking the service into operation begins with somebody making up a value.
