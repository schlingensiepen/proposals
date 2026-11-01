---
type: Proposal Rule
description: Behavior that clings to one operation stands at the operation, the usage description names only the principle, no rule is spelled out in both places, and a check in the build holds the separation.
number: F22
binding: must
role: load-bearing
applies_to: [builder]
---

# F22 · Principle and case: every behavior rule stands exactly once, held by machine

**Applies to:** builder.

**What holds.** Behavior that clings to a single operation stands at the operation (F6; see the two kinds of description in the concept); the usage description carries only overarching principles. No rule is spelled out in both places: the usage description names the *principle* exactly once ("actions that take effect are confirmed by the user"), the operation names its *case* ("this call files the message; obtain confirmation first"). This separation is held by a check in the build that fails as soon as the usage description takes in operation behavior; a rejected attempt is documented in the check itself, so that the next attempt sees that it has been made before.

**Why.** The separation is not aesthetic but the only protection against two descriptions of the same behavior, and two spelled-out versions of the same rule drift apart; the early indicator is a contradiction in a detail (F1). It has to be held by machine, because it is always violated for a good reason: the sentence that wants into the usage description is useful every time, and precisely for that reason an intention does not stop it. Without the check, the usage description would have become a second interface description that goes stale with the next rebuild.

**Obvious and rejected.** *"Better in both places, then everybody finds it"*: then everybody finds two versions, and one of them is wrong at some point. *Holding the separation in review*: reviews do not recognize the single useful sentence as a violation, because it is useful. *The check as a mere word list*: too coarse and too fine at the same time; it needs a judgment about what operation behavior is, and this judgment is made once, written down and then checked.

**How you notice it is violated.** The usage description grows by a paragraph with every new operation, or after a rebuild it describes a behavior that the operation has long documented differently.
