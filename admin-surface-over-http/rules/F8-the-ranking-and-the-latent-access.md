---
type: Proposal Rule
description: The duties the imported where-a-secret-comes-from places on any ranking apply to the administration secret unchanged; what this extension adds is that the surface's entry page and the deployment file say that changing it is two moves.
number: F8
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F8 · The ranking makes replacing easy and deleting invisible: the latent access

**Applies to:** builder · operator.

**What holds.** The duties `where-a-secret-comes-from` places on any ranking apply to the administration secret unchanged: the higher rank wins but never hides (the service reports lower ranks that are also set at start, with their place and how to clear them), and a change is two moves, replacing the higher rank **and** clearing the lower one, with a way without a shell for the file the service wrote itself (in the pattern: set `new` and restart). **What this extension adds:** the entry page of the administration surface (F11) and the deployment file that switches it on say that changing the administration secret is two moves and not one.

**Why.** For this root, the latent access has happened: on a change the environment variable was replaced, the secret generated at first start stayed in the store and would have applied again as soon as the variable was missing once, when the stack moves or is re-created from a template. Nobody knew its value. The reasoning behind the report and the two moves is in the imported concept; what is added here is that the operator without a shell has the entry page at the port and the deployment file in front of him and nothing else, so that is where it must be said.

**Obvious and rejected.** *Silently ignoring the lower rank while the upper one applies*: the state as built, and the incident.

**How you notice it is violated.** A removed administration secret keeps working because a second one lay underneath the removed one; or an operator cannot say how many valid administration secrets his instance has right now.
