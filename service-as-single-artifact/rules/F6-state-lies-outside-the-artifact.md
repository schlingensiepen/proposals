---
type: Proposal Rule
description: Everything that must survive a restart, configuration and state, is kept at named, separate places outside the artifact, so a version change carries nothing along and leaves nothing behind.
number: F6
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F6 · The state lies outside the artifact

**Applies to:** builder · operator.

**What holds.** Everything that must survive a restart is kept at named places outside the artifact: the configuration, which is set before the start, and the state, store, operating data, queues, which arises in operation (F12); both at separate places (F8). The artifact is interchangeable: a change of version takes nothing along and leaves nothing behind.

**Why.** Otherwise "one delivery artifact" is an assertion: an update would destroy state, a rollback would lose data, and nobody could take a version back without danger. The interchangeability of the artifact is the practical return on the whole design.

**Considered and rejected.** *The data store in the image* — convenient at the first start, fatal at the second. *Compiling the configuration in* — every change becomes a rebuild, every environment its own delivery artifact (F4). *State in the working directory without a named place* — works until someone changes the process form.

**How you notice it is violated.** A change of version requires moving data by hand — or a rollback loses something that was there before.
