---
type: Proposal Rule
description: A set but unusable administration secret aborts the start instead of a silent fallback to a generated one, as the imported where-a-secret-comes-from allows; here also an unwritable or unreadable file and an unbindable administration port.
number: F7
binding: must
role: load-bearing
applies_to: [builder]
---

# F7 · No silent fallback: a set but unusable secret aborts the start

**Applies to:** builder.

**What holds.** For the administration secret, the choice "set but empty or unusable" of `where-a-secret-comes-from` is decided as abort: if it is set, in the variable or in the file, but unusable (in the pattern: shorter than 32 characters, a deliberately coarse floor against typed short words, not a measure of entropy), the service aborts the start and says why and what to do; it does **not** silently generate one of its own. **What this extension adds:** the same holds for the failure modes around this surface. If the file cannot be written or cannot be read, or the administration port cannot be bound, the start aborts, instead of the service running without administration or with a secret other than the one ordered.

**Why.** A fallback to a generated secret would be the most dangerous thing the service could do here: the operator believes his value applies, and the service runs under another. Two truths about the root, and the wrong one with the human. An aborted start costs one round and one message; a silent fallback costs trust in every later statement about this secret.

**Considered and rejected.** *Accepting a secret that is too short, "the operator will know what he is doing"*: he typed a word. *Generating on an unusable value and logging both*: the exception to the rule against secrets in logs would be triggered for a case the operator did not order. *Running on without administration when the port is taken*: the operator without a shell cannot get in and learns it from an error of the proxy.

**How you notice it is violated.** An operator sets a secret, and another one applies; or the service runs and the administration port does not answer, without the start having said so.
