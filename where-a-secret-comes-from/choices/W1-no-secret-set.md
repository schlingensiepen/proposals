---
type: Proposal Choice
description: What the service does when no rank holds a secret: refuse to start, or generate it and show it exactly once over the once-only channel.
number: W1
granularity: per-setting
applies_to: [builder, operator]
---

# W1 · No secret set

**The choice.** What the service does when no rank holds a secret.
Decided per secret, in practice per group, by the builder together with
the operator (or whoever stands in for them).

**(a) Refuse to start.** The start aborts and says which secret is
missing and under which name it is expected. Consequence: there is no
once-only channel; [F6](../rules/F6-never-in-any-output.md) has no
exception. Fits everything the service cannot generate (inherited keys
to target systems) and everything it should not generate (accesses to
the function).

**(b) Generate and show exactly once.** The service generates the secret
(high entropy, recognisable prefix), stores it at rank 2 and shows it
exactly once over the once-only channel: for a service one log line of the form
`<SECRET NAME> generated: <value>. Shown once; it will not be shown
again.`; for an agent the context
window, persisted immediately, never repeated. Consequences: never at an
ordinary restart (only when the command word of
[F5](../rules/F5-replacing-is-two-moves.md) is present), never for a
secret somebody set; a smoke test in the build
verifies that it appears exactly once, is accepted, still applies after
a restart and does not appear again; F6 gets its named exception.
Condition: only for a secret whose only possible owner is the service;
when an image is pulled, nobody but the service is inside.

**Warning for (b).** Whoever takes this path must give the holder of the
generated secret enough rights to finish setting up the system: whoever
has only that one log line must be able to get in with it. A secret
that suffices for that is a root. What a root owes, revocation, change,
lifetime, the case of loss, is for whoever introduces it to state, not
for this concept.

**Rejected for all options.** *Shipping a first secret with the image*:
it is in every copy. *Generating at first access and handing it to the
caller*: whoever happens to call first gets it. *Showing it again after
loss*: every re-display is a query surface; loss means replacement.
*"The service makes one up" for everything*: it cannot generate
inherited keys, and it should not generate accesses to the function.

**Record your decision.** Per group; inherited keys are always (a).
Accesses to the function are not start secrets at all; the service
issues them in operation, and this choice does not apply to them.
