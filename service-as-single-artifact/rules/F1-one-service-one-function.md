---
type: Proposal Rule
description: A service carries exactly one function, describable in one sentence; main and helper methods serve it and share one access; a second independent function is a second service.
number: F1
binding: must
role: load-bearing
applies_to: [builder]
---

# F1 · One service, one function

**Applies to:** builder.

**What holds.** The service carries exactly one function; it is its purpose and describable in one sentence. The network surface splits it into methods: a main method and the helper methods without which it does not work (preparation, status enquiry, cleaning up what was prepared). All methods serve the same function and share the same access. Whoever needs a second, independent function builds a second service.

**Why.** The one function is the precondition for everything else staying small: a service with exactly one function needs no distinction as to who may use which method — and hence neither roles nor permission management nor someone to run them (F9, F10). It is moreover describable in one sentence, for humans as for agents, and its description stays readable instead of turning into a catalog.

**Obvious and rejected.** *One service for everything ("since it is running anyway")* — as soon as two independent functions lie under one access, a permission distinction is needed, and with it roles and their administration; saving one delivery does not outweigh that burden. *One access per method* — then the caller juggles several accesses in order to upload a file and use it; the ability is the function, not the individual call. *The helper methods as small services of their own* — distributes one workflow across several deliveries and produces exactly the coordination problems this dossier avoids.

**How you notice it is violated.** One is discussing whether a caller should be allowed one method and not the other — or the service can no longer be described in one sentence without saying "and also".
