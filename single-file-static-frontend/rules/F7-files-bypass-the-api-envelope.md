---
type: Proposal Rule
description: Anything that does not fit through the API protocol's envelope should get a route of its own, a separate intake for uploads and direct delivery by the file server for downloads; how exactly depends on the chosen API technology.
number: F7
binding: should
role: independent
applies_to: [builder]
---

# F7 · Files and binary data bypass the API envelope — recommendation

**Applies to:** builder.

**What holds.** Anything that does not fit through the API protocol's envelope — images, attachments, archives — should get a route of its own: a separate intake for uploading and direct delivery by the file server for downloading. **How** exactly depends on the chosen API technology and is to be decided there.

**Why.** A protocol with a fixed envelope — of whatever kind — transports bytes only encoded, and every encoding costs volume and forces the data into memory in full. This is not an implementation detail: it decides whether memory demand grows with the **number of requests** or with the **size of the payload**. Whoever notices only when the first large file arrives has to change the interface.

This is a **recommendation**, not a requirement: this cut does not commit to an API technology. Whoever picks one should answer this question before the first file, not after.

**Obvious and rejected.** *Send it encoded for now and change it later* — the change hits client and server at once and is therefore the most expensive kind. *Serve files through the application to check permissions* — then application code runs for every thumbnail, and F3 is abandoned for the bulk of the traffic.

**How you notice it is violated.** The server's memory demand depends on payload size rather than on the number of requests.
