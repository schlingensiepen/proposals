---
type: Proposal Rule
description: Changing a secret means replacing the higher rank and clearing the lower one; the operator has a way for that without a shell, and the service never deletes a secret on its own.
number: F5
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F5 · Replacing is two moves

**What holds.** Changing a secret means: replace the higher rank
**and** clear the lower one. Where the lower rank is a file the service
wrote itself ([W1 (b)](../choices/W1-no-secret-set.md)), the operator
has a way to do that without a shell: an agreed command word in the
variable (say `new`) and a restart; the service then replaces the file,
shows the new secret once, and keeps doing so at every start as long as
the command word is there, which the start report says. The command
word is a command, not a secret; it is exempt from the usability check
of [W2](../choices/W2-set-but-empty-or-unusable.md). Where the lower
rank is a file the operator placed (an inherited key), the service
cannot clear it; the start report names the path, and the operator
clears it the way their platform allows. The usage description says
that it is two moves. The service never deletes a secret on its own.

**Why.** Without the second move the latent access
([F4](./F4-the-higher-rank-wins-and-never-hides.md)) stays a piece of
knowledge but not an action: the operator sees the report and can do
nothing without a shell. And a service that deletes on its own takes
the decision about a secret they may have written down away from the
operator.

**Considered and rejected.** *"Changing the variable is enough"*: the
lower one is still there. *Automatic cleanup at start*: deletes what
the operator may still need.

**How you notice it is violated.** There is no way without a shell to
replace a secret the service generated; or after the change the old one
still applies.
