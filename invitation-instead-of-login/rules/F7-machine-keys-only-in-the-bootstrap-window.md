---
type: Proposal Rule
description: Capability keys are created only while a bootstrap window is open, their clear text shown once; revoking them is possible at any time in normal operation.
number: F7
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F7 · Machine accesses come into being only in the bootstrap window

**Applies to:** builder, operator.

**What holds.** A capability key **cannot** be created during normal operation, not even by an administrator. It comes into being exclusively while a bootstrap window is open (F8). Its clear text appears **exactly once**; afterwards the system holds only a hash.

**The window opens at every deployment, not only the first.** This makes the rule no restriction in daily life but a scheduling rule: whoever connects a new machine deploys and has its keys created in the same move; the person setting up holds the setup secret anyway while deploying. There is no case of "I need a key now and cannot get one"; there is only "I need a key, so I deploy".

**Why.** An administrative credential sits on a device for months. If it could issue a machine key, a single captured device would suffice to obtain a **permanent** access, one that survives the device being blocked and, in the worst case, hands out the entire dataset. Creating permanent machine accesses therefore belongs behind the same threshold as the bootstrap itself.

The second reason is an attitude that pays off: **a machine access is a deliberate act, not a by-product.** An administrator wanting to back up by hand creates a key for it, rather than such access running along as a silent side-right of a role.

**Creating is time-limited, revoking is always available.** This asymmetry is deliberate and must be stated, because it does not follow by itself: a capability key can be **shut down during normal operation**, without a window, without a deployment, without the setup secret; an administrator suffices. Otherwise a compromised computer would keep running until the next deployment, and the block would come too late exactly when it is needed.

The reasoning is the same as with devices (F4): a procedure that **takes rights away** must never be harder to reach than the one that grants them. When granting, you may build hurdles; when revoking, every hurdle costs time in which the damage continues. That is why every key carries the name of its machine and its task (F6); otherwise, in an emergency, nobody knows which of five keys to shut down.

**Obvious and rejected.** *Leave creation in the administration area*: convenient, and it turns a captured administrative credential into a master key of unlimited durability. *Bind revoking to the window too, "for symmetry"*: sounds consistent and is the mistake: a compromised computer would then keep running until the next deployment. *Store the clear text, "so it can be looked at again"*: it is then a permanently readable secret, and the one-time appearance was for nothing.

**How you notice it is violated.** A machine key can be displayed in clear afterwards, or created from within the running application.
