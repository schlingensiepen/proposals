---
type: Proposal Rule
description: The order in which components are delivered is a technical dependency, not an agreement; whoever can only get their secret while the window is open is delivered after the one who opens it, the tool enforces this, and it matters only on the first delivery per instance.
number: F7
binding: must
role: load-bearing
applies_to: [builder]
---

# F7 · The order follows the flow of secrets, not a convention

**Applies to:** builder.

**What holds.** The order in which the components are delivered is **not an agreement** but a technical dependency: whoever can only get their secret while the window is open is delivered after the one who opens the window. The tool enforces the order instead of documenting it.

**Why.** A documented order is one that has to be remembered, and the failure is inconspicuous: the component delivered the wrong way round runs, it just has no access and reports that only on its next scheduled run, possibly at night. **This is the actual yield of this concept:** take F2 and F6 together, and the order arises by itself; one does not have to invent it, only read it off.

**Only the first time per instance.** Afterwards the plain text lies locally and the order no longer matters; the dependency exists between *creation* and *receipt*, not between the components as such.

**Obvious and rejected.** *Writing the order into the guide*: it is read when something has gone wrong, not before. *Every component waits until its access is there*: shifts an order into a waiting loop and creates a state that somebody has to end.

**How you notice it is violated.** A guide contains a sentence of the form "deliver A first, then B", but nothing enforces it.
