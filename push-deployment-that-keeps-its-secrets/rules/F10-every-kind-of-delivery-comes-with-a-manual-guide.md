---
type: Proposal Rule
description: For every component and every transfer path there is also a guide describing the same by hand, first installation and migration of the store included; it is part of the delivery, not an extra.
number: F10
binding: must
role: independent
applies_to: [builder, operator]
---

# F10 · Every kind of delivery comes with a guide for manual operation

**Applies to:** builder, operator.

**What holds.** For every component and every transfer path there is in addition a guide that describes **the same by hand**, first installation and migration of the store included. It is part of the delivery, not an extra.

**Why.** The script presupposes things that do not hold everywhere: a runtime environment, a transfer path, a network connection at the right moment. If one of them fails, the operator faces a system they cannot deliver, and improvises. **That is exactly when the mistakes arise that the script is meant to prevent**, only without its locks and without its check.

The pair reads: **use the script, and if that does not work, follow the guide.** Both have to exist; otherwise it is not a pair but a hope.

**A side effect that justifies the guide even if it is never needed:** whoever writes it has to name what the script does. An operation that cannot be described in manual steps is usually not cleanly separated in the script either.

**Obvious and rejected.** *Doing without the guide, "the script is documented after all"*: source code is not a guide, and in a fault nobody reads it. *One shared guide for all paths*: the paths differ precisely in the manual steps that matter.

**How you notice it is violated.** To the question "how do I do this without the script?" there is no answer other than "look into the script".
