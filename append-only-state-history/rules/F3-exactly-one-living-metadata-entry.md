---
type: Proposal Rule
description: One machine-added entry may arise and change after the fact on the current latest state only, and is frozen as soon as a further state follows; there is one such entry, not several.
number: F3
binding: must
role: load-bearing
applies_to: [builder]
---

# F3 · Exactly one living metadata entry, and only on the latest state

**What holds.** One piece of information added by a machine may come into being after the fact on the **current latest** state and may change there. As soon as a further state follows, it is frozen. There is **one** such entry, not several.

**Why.** Machine evaluation takes time and lags behind the input. Without this exception, every result would have to create a new state, and the history would consist of entries that no human initiated. The limit "only on the latest" is what keeps the exception small: what has once been superseded is never touched again.

**Obvious and rejected.** *Allowing several living entries*: each one on its own can be justified, and together they cancel F1. *Evaluation results as states of their own*: the history fills up with events that nobody initiated.

**How you notice it is violated.** A state that is no longer the latest has changed.
