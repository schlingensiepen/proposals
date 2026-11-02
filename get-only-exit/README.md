---
type: Proposal
description: A named emergency exit for callers that can only send GET without headers of their own; the same operations under their own prefix, everything in the path with the secret first, writing as small fetches of which exactly one takes effect, and the price, the secret in the address, named at the door.
version: "1.1"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - one-operation-many-surfaces
---

# Emergency exit for GET-only callers

This directory is an OKF bundle for one proposal of the series: how a
service makes itself callable for tools that can fetch an address and
nothing else, without spoiling its real interface. It is a base concept
and a deliberate, named exception: the author calls the result "the
ultimate hack" and "an anti-API".

**Question.** My agents reach my service with tools that can only send
GET: no header of their own, no request body. How do I make the service
callable for them without spoiling the real interface?

**Thesis.** If you want to reach callers that can only send GET
without headers of their own, you build a named emergency exit beside the
main surface: the same operations under their own prefix, everything in
the path with the secret as the first segment, writing as a sequence of
small fetches of which exactly one takes effect. And you name its price,
the secret in the address, at the door; the main surface never offers
this way.

## Start here

The [concept](./concept.md) describes the problem, how the exit relates to the rest of a self-describing service, its load-bearing structure and the twelve-step procedure; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Twelve rules in four blocks: the design F1 to F3, the operations F4 to F7, behavior on error and repetition F8 to F10, price and description F11 and F12.

* [F1 An emergency exit, not a second API](./rules/F1-an-emergency-exit-not-a-second-api.md)
* [F2 Everything in the path, nothing in the query; the secret first](./rules/F2-everything-in-the-path-the-secret-first.md)
* [F3 Two encoding rules](./rules/F3-two-encoding-rules.md)
* [F4 Reading mirrors the main surface; the form follows the caller](./rules/F4-reading-mirrors-the-main-surface.md)
* [F5 The upload in parts](./rules/F5-the-upload-in-parts.md)
* [F6 The composition](./rules/F6-the-composition.md)
* [F7 The word: composition, not draft](./rules/F7-the-word-composition-not-draft.md)
* [F8 Refusals that work on this surface](./rules/F8-refusals-that-work-on-this-surface.md)
* [F9 No guessing](./rules/F9-no-guessing.md)
* [F10 Repeatability without headers](./rules/F10-repeatability-without-headers.md)
* [F11 The price, named](./rules/F11-the-price-named.md)
* [F12 The description is the only document its caller has](./rules/F12-the-description-is-the-only-document.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled; the [evidence](./evidence.md) shows the reference implementation, what it proves and what not, and the honest state. The [sources](./sources.md) name one import, four tensions and related proposals; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) serves an agent that builds or reviews such a surface, or has to use one as a consumer; the [example](./examples/walkthrough-message-with-attachment.md) follows a reviewing agent through the surface step by step; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed
CC BY 4.0; the license text and the register of all proposals are in the
root README of the repository.
