---
type: Proposal Rule
description: Accesses one component needs for another are not created in running operation but in a window that the delivery opens and that closes by itself after a fixed period; their plain text appears exactly once, on the receiving side and never in the terminal, and revoking them stays possible at any time without a window.
number: F6
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F6 · Accesses for other components arise in a time-limited window

**Applies to:** builder, operator.

**What holds.** Accesses that one component needs **for another** are not created in running operation but in a window that the delivery opens and that closes by itself after a fixed period. Their plain text appears **exactly once**, on the receiving side, and never in the terminal.

**Why.** Such accesses are permanent and powerful: a backup access hands out the entire store. If they could be created at any time from the running application, one captured administration access would suffice to obtain one that survives the administrator being locked out. The window moves this operation to the only place where somebody with full access is acting anyway: the delivery.

**The trust boundary does not run at the window.** Anyone who can open it can write to the host system; they could replace the application. The window gives them nothing they did not already have; it only makes it **conveniently reachable for a limited time**. In practice a small file beside the configuration suffices, holding a secret and an expiry time, checked by a comparison with the clock: no timer, no background process.

**And the opposite direction is explicitly different:** such an access can be revoked **at any time** in running operation, without a window and without a delivery. An operation that takes rights away must never be harder to reach than the one that grants them.

**Considered and rejected.** *Leaving the creation in the administration area*: convenient, and it turns a captured administration access into a master key with unlimited shelf life. *A file that one deletes after the setup*: relies on somebody remembering; it is forgotten in exactly the cases where it would matter. *Binding revocation to the window as well*: sounds consistent and lets a taken-over component keep running until the next delivery.

**How you notice it is violated.** A machine access can be created from within the running application, or can only be revoked while the window is open.
