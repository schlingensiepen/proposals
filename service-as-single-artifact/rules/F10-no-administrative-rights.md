---
type: Proposal Rule
description: There is no administrative user, access or permission model; administration is configuration, and no issued access carries administrative power.
number: F10
binding: must
role: load-bearing
applies_to: [builder]
---

# F10 · The service knows no administrative rights

**Applies to:** builder.

**What holds.** There is no administrative user, no administrative access and no permission model for administration. Administration is configuration, and anyone who may change the configuration administers — enforced through file permissions (F8). No issued access ever carries administrative power; it carries the function, nothing else. This does not cover the domain model of a service whose *function* is the administration of accounts or permissions: what it offers as its function, it offers over its network surface (F1).

**Why.** A permission model is expensive — it wants to be designed, checked, documented and maintained in operation — and for a service with one function and preconfigured accesses it has no object. Where there are no administrative rights, nobody can inherit them, extend them or forget to withdraw them.

**Considered and rejected.** *An administration token "for emergencies"* — precisely the artifact whose loss costs everything, and it is guaranteed to be lying around somewhere. *Roles in the service for operational tasks* — introduces the very permission management this design saves, and turns the network surface back into an administration path (F7).

**How you notice it is violated.** There exists an access with which one can do more than the one function.
