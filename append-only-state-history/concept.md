---
type: Proposal Concept
description: The problem of losing nothing and duplicating nothing, the concept of an immutable sequence of states with attachments carried over by reference and one narrow exception, its load-bearing structure; the procedure is not yet written.
---

# The concept

**In short.** Where traceability matters, a mutable record is the root problem: in the end one sees only the latest state and does not know what held when. An immutable sequence of states provides both, at the price that attachments must not be copied but carried over, and that there is exactly one justified exception.

## Problem

"I photograph the contents of a box, somebody later adds a description, half a year after that an item is missing. If I overwrite the record every time, only the latest state is left in the end, and the question 'when did that disappear?' can no longer be answered. If instead I file every change as a complete copy, the photos double with every trifle."

Two requirements that seem to contradict each other: **lose nothing** and **duplicate nothing**.

## The concept

The item itself carries no data; it is only an identity. Everything of substance belongs to a **state**: a snapshot with a point in time and a creator. A change does not change a state but **creates a new one**; the old one stays as it is. Attachments, images and files, are carried over into the new state **by reference**, not copied: the same attachment can occur in many states and still exists only once.

So everything is immutable, **with exactly one exception**: one metadata entry that a machine adds after the fact and that is not yet final may live on in the **current latest** state until the next state freezes it.

**Load-bearing structure.** F1 carries everything. **F2 is the condition for F1 staying affordable**: without carrying over by reference, immutability is given up at the first large attachment. F3 is the only exception to F1 and is acceptable only because it is narrow. F4 is independent.

> **Reservation.** F1 to F4 were developed on a flat model and tried out there. How they carry over to object trees is **not** decided; see what is not yet settled in the scope.

## The procedure

Not yet written in version 0.3.
