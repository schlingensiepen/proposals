---
type: Proposal Rule
description: If a secret is set at several ranks the higher one applies, and the service reports at start that a lower rank is also set, where it is and how to clear it.
number: F4
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F4 · The higher rank wins and never hides

**What holds.** If a secret is set at several ranks, the higher one
applies. The service reports at start that a lower rank is also set,
says **where** it is and **how** to clear it (the start report has at
least: secret name, rank in effect, lower ranks also set with their
path, and the way to clear each). A set higher rank never
silently hides a lower one.

**Why.** A ranking makes replacing easy and deleting invisible:
replacing means setting something at the top, and that takes effect at
once; deleting would mean removing something at the bottom, and nobody
sees that because something applies at the top. The incident behind
this rule has happened: a secret generated at first start stayed
underneath the changed variable, known to nobody, and would have
applied again the next time the variable was missing. A latent access.
Anyone who introduces the ranking pays for it with the report.

**Considered and rejected.** *Silently ignoring the lower rank while the
upper applies*: the state as built, and the incident. *Deleting the
file as soon as the variable applies*: an operator who sets the
variable only temporarily loses their only stored secret; a service does
not delete secrets on its own.

**How you notice it is violated.** A removed secret still applies
because a second one was still underneath; or an operator cannot say how many
valid secrets their instance has right now.
