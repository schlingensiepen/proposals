---
type: Proposal Rule
description: The model prescribes no rights; it provides two anchors, user and user on device, and demands five properties of any rights system built on them.
number: F10
binding: must
role: load-bearing
applies_to: [builder]
---

# F10 · The model provides two anchors; the application hangs its rights on them

**Applies to:** builder.

**What holds.** This model prescribes **no rights**. It is the substructure of a domain application, and only that application knows what an invitee may do. What is provided are **two anchors** to which rights can be attached:

| Anchor | carries |
|---|---|
| **User** | everything that applies to the **person**: domain rights, memberships, mail address |
| **User on device** | everything that applies to the **single access**: label, origin, revocation, last use |

To these anchors the application ties its own rights system. This concept demands **five properties** of it, not because they are required by the domain, but because the access model does not hold without them:

1. **The model's own procedures are themselves subject to rights.** Inviting, withdrawing an invitation, revoking a device, deactivating a user: each needs a right. Without that, every invitee may invite further, and belonging is no longer a decision but a chain reaction (F2).
2. **Domain rights hang on the user, not on the device.** Otherwise a right disappears when a device is revoked, and with three devices the pointless question arises which is "the one with the right".
3. **Revoking one's own device requires no special right.** That is signing out. It is the only procedure the affected person can trigger **themselves** when a device has gone missing and nobody is reachable; without it, you must find somebody at the worst possible moment.
4. **Revoking is never harder to reach than granting.** The same asymmetry as with capability keys (F7): when granting you may build hurdles, when taking away every hurdle costs time in which the damage continues.
5. **The last administrator cannot take the administrative right from themselves.** Otherwise the system locks itself out, and the only way back leads through the bootstrap, that is, through a deployment (F8). The rule is cheap, its absence is expensive.

**Why.** An access model that also prescribes rights is almost always wrong in a domain application: what somebody may do follows from the subject matter, not from the way they came in. Conversely the model must not leave the question **entirely** open, because four of its own procedures, inviting, withdrawing, revoking, deactivating, exist only through this model and would otherwise have no owner. The dividing line is therefore: **the design of the domain rights is open, the responsibility for the model's own procedures is not.**

The five properties are not rights but **statements about any rights system** built on top. They can be satisfied with two roles just as well as with fifty.

**Considered and rejected.** *Ship a rights model* ("reader, writer, administrator"): convenient, and wrong in the second domain application; you inherit roles that do not fit the subject and build your own beside them. *Leave the question entirely open*: then nobody owns revoking, and it gets improvised at the first incident. *Treat inviting as a "small" right beside administering*: anyone who may invite grants access to the entire dataset; it is thus as powerful as administering, and granting it separately demands a justification of what an inviter may **not** do. *Rights on the device instead of the user*: looks like fine-grained control ("administer only on the home computer") and doubles the state you must keep consistent (F5).

**How you notice it is violated.** Somebody has lost their device and must wait until somebody else can revoke it, or a right has vanished along with a revoked device.
