---
type: Proposal
description: The frontend is one artifact, an HTML file with its vendored assets, delivered without authentication and therefore allowed to contain no data; everything of substance it fetches at runtime through an API.
version: "1.0"
date: 2026-08-15
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# One file anyone may have

This directory is an OKF bundle for one proposal of the series: how to cut a browser application so that deploying it stays a copy operation, and it still gives nothing away. It is a base concept; it commits to no API technology.

**Question.** How do I cut an application so that deploying it stays a copy operation — and it still gives nothing away?

**Thesis.** The frontend is a single static file anyone may have; the address names no resource on the server but a state of the client.

**In short.** Whoever assembles a frontend from a build result, a server template and a routing configuration deploys three things that must match. A single static artifact turns deployment into a copy operation — and because anyone may have it, nothing may be in it that anyone should not have. The price is uglier addresses, no server-side preparation, and an empty first screen until the first response arrives.

## Start here

The [concept](./concept.md) describes the problem and the single static artifact, with the procedures for setting up and for reviewing; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Eight rules; F7 and F8 are recommendations, because they concern the interface, to which this cut does not commit. Whoever takes only one rule takes F2.

* [F1 One artifact, not a build result](./rules/F1-one-artifact-not-a-build-result.md)
* [F2 The file is delivered unauthenticated, and therefore carries no data](./rules/F2-delivered-unauthenticated-carries-no-data.md)
* [F3 No application code runs in the delivery path](./rules/F3-no-application-code-in-the-delivery-path.md)
* [F4 The address names a state of the client, not a resource of the server](./rules/F4-the-address-names-a-client-state.md)
* [F5 The state sits in the fragment, so the server need not be bent](./rules/F5-the-state-sits-in-the-fragment.md)
* [F6 Third-party assets are vendored, not linked](./rules/F6-third-party-assets-are-vendored.md)
* [F7 Files and binary data bypass the API envelope (recommendation)](./rules/F7-files-bypass-the-api-envelope.md)
* [F8 Delivery and API under one origin (recommendation)](./rules/F8-delivery-and-api-under-one-origin.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when the answer flips, what it does not solve and what is not yet settled; the [evidence](./evidence.md) gives an invented walk-through of an inventory application and says why there is no reference implementation yet. The [sources](./sources.md) name related proposals and external sources; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for building or reviewing such a frontend; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
