---
type: Proposal Rule
description: Command line and administration surface are pass-throughs onto one operation under the imported one-operation-many-surfaces; what this extension adds is that neither surface calls the other and that the machine-readable output of the command is the structure of the administration surface.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · One operation, two pass-throughs: no surface has logic of its own

**Applies to:** builder.

**What holds.** The command line and the administration surface are two surfaces in the sense of `one-operation-many-surfaces`, and that concept applies to them unchanged: every administrative action exists exactly once as a callable function that knows neither HTTP nor process exit; both surfaces are pass-throughs onto it, each with exactly one translation function (status codes here, exit codes there) and the same field names; no surface writes the store, touches the file system or talks to the target system. **What this extension adds:** neither surface calls the other; the command needs neither the administration secret nor a running service. And because the administration surface replaces the command completely (F1), the machine-readable output of the command is the structure of the administration surface, and every route mirrors a command.

**Why.** The imported concept justifies why every operation exists exactly once and every surface is a pass-through onto it. What is said here is only what these two surfaces add: the same field names are what "replaces the command completely" means for a script; it can learn on one surface what it reads on the other. And the shell keeps its advantage, authentication by server access, only as long as the command does not go through the administration surface.

**Considered and rejected.** *The HTTP surface first, and the command line calls it over the network*: then the command needs the administration secret, a running service and a port, and the shell loses exactly the advantage it has, authentication by server access. *Building one surface "more completely" because one is at it anyway*: then it is no longer a pass-through and the other is incomplete (F1).

**How you notice it is violated.** The same administrative action behaves differently on the command line and the administration surface and nobody decided it; or a field has a different name on one surface than on the other; or the command needs the administration secret or a running service.
