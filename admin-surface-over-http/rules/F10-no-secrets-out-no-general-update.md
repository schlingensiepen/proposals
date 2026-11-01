---
type: Proposal Rule
description: Secrets flow into the administration surface only; an issued access appears once, with the address it is valid for, and listings never show a secret; there is no general update of a variant, only create, remove, set the third-system password and rotate.
number: F10
binding: must
role: load-bearing
applies_to: [builder]
---

# F10 · The surface gives out no secrets and knows no general update

**Applies to:** builder.

**What holds.** Secrets flow over the administration surface **only inward**: an issued access appears once in the answer to its creation or renewal, together with the address under which it is valid (the user-facing surface runs on another port under another name, and whoever knows only the administration cannot infer it from its description), and never otherwise; listings show identifier, name, target and last use, never a secret, not even stored credentials for third systems. And the surface offers **no general update** of a variant: there is create, remove, setting the third-system password and renewing the access; whoever wants to change target, address or procedure removes and creates anew and gets a new access. Both hold for both pass-throughs, because they are properties of the operations (F2).

**Why.** Both follow from F9: a root on the network is bearable only if what it reaches is small. An administration that can display what exists turns a captured administration access into a key ring; one that gives out only at creation does not; the loss is small because a lost access is renewed instead of looked up. The general update is the least conspicuous attack on a service that keeps credentials for third systems, and over the network it is reachable: whoever bends the target address of an existing entry to a server of their own does not need to read out the stored password; the service logs in there at the next operation and **delivers it along**, and a willing counter-server confirms every operation, so nothing fails. A check rule ("whoever changes the address must set the password anew") would catch that and be forgotten at the next field; a **missing function** cannot be bypassed. Setting the password remains as the only change, because expiring credentials are the most frequent legitimate case and a password change cannot redirect anything. That the base cut already knows no update for another reason (a changed variant is a new access) does not make this rule superfluous: there it is a question of the cut, here one of attack surface, and a builder retrofitting the surface would obviously reintroduce the update "for administration".

**Obvious and rejected.** *A full update with check rules*: the rule is right and will be forgotten at the next field. *Logging changes instead of forbidding them*: visibility replaces no lock when the damage occurs in the same instant. *Displaying existing accesses "so the operator can give them to a new client"*: renewal is there for that; the display turns the root into a key ring. *Returning the issued token without the address of the user-facing surface*: the operator without a shell then has a secret and does not know where to take it; exactly that happened and was fixed.

**How you notice it is violated.** There is a call that returns an existing secret; or an administrative call can change the target of a stored login without asking for the credentials anew; or an operator asks where to send the token just issued.
