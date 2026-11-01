---
type: Proposal Rule
description: A human gets a credential pointing at them; a machine gets a key permitting one task at one place and representing nobody, one key per task and per machine.
number: F6
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F6 · Humans receive identity, machines receive capability

**Applies to:** builder, operator.

**What holds.** A human receives a credential pointing at them; what they may do is looked up (F5). A machine receives a key permitting **one task**, backing up or analyzing, and **representing nobody**. One key per task, one per machine.

**Why.** The difference is not cosmetic. An identity credential asks "who are you?" and looks up afterwards; a capability key carries its permission within and asks nobody. That matches what the two actually are: a human changes devices, gains and loses rights, has an address at which they can be reached. A background process has none of that; it has exactly one task and runs until somebody switches it off.

Give a machine a user account and an **identity that belongs to nobody** comes into being: unlimited in time, without an address, without anyone who would miss it if it were misused, and with all the rights it was given for convenience. Conversely, a capability key is wrong for a human, because you cannot tell who used it.

**The capability side in its strict form means: one key, one capability, one place.** No parameter with which the caller chooses their scope: the key **is** the scope. Whoever has two tasks gets two keys; whoever works at two places likewise. There is then no authorization code that could be wrong, and a revocation hits exactly one capability at exactly one place. How such a key is to be handled beyond that, handover, storage, rotation, expiry, is a subject of its own; see the sources.

**Separated per task and per machine**, because otherwise the incident costs everything: the backup access hands out the **entire** dataset at once; the analysis access sees only what it is currently processing. Merging those two into one key saves a line of configuration and costs, in an incident, the difference between them. And the key's name carries its machine, so a compromised computer can be shut down **individually**, the same argument as in F4.

**Obvious and rejected.** *A technical user for the service*: the most common route, and it produces exactly the ownerless identity above. *One key for all machine tasks*: saves configuration and costs everything in an incident. *Humans via capability keys too*: then it is no longer determinable who did something, and the history becomes worthless.

**How you notice it is violated.** The user list contains an entry belonging to nobody, or a single key opens both the analysis and the entire dataset.
