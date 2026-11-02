---
type: Proposal Rule
description: The server knows exactly one entry point and always delivers the same document; the address expresses a state of the client and names nothing the server could resolve.
number: F4
binding: must
role: load-bearing
applies_to: [builder]
---

# F4 · The address names a state of the client, not a resource of the server

**Applies to:** builder.

**What holds.** The server knows exactly **one** entry point and always delivers the same document. Which view the application shows is the client's business; the address expresses that state but names nothing the server could resolve.

**Why.** This is the actual shift of this design, and it is often missed: in a server-side application the address **is** a resource — the server looks it up and answers. Here there is nothing to look up; there is a document, and within it a state. Anyone who keeps thinking the address is a resource inevitably builds a server that has to pretend it is — and that is exactly where the configuration comes from that this concept means to avoid (F5).

The consequence is also what **cannot** be done: there is no address under which the server could deliver a partial view, no server-side pagination, no partial page for a request without JavaScript.

**Considered and rejected.** *Carry the views as real paths and have the server resolve them* — then it is no longer a single-file application but a server-side one with an addition. *One HTML file per view* — solves the address problem and brings back everything F1 avoids: shared parts must be kept in sync, so you need a build step or copies.

**How you notice it is violated.** The server has to know which views the application has.
