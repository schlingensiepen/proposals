---
type: Proposal
description: Every administrative action exists exactly once as an operation that knows no surface, every surface of the service is a pass-through onto it with only translation and presentation of its own, and a structural test in the build holds that, because two correct copies pass every behavioral test.
version: "0.1"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# One operation, many surfaces

This directory is an OKF bundle for one proposal of the series: a base concept for every service that can be reached over more than one surface (a command line, an HTTP surface, further ones). It says how the surfaces stay one service instead of becoming two implementations that drift apart.

**Question.** My service has more than one surface: a command and, beside it, a second one, over HTTP for instance. How do I keep the same administrative action from being implemented on each surface separately, with the implementations drifting apart without any test noticing?

**Thesis.** Every administrative action exists exactly once, as an operation that knows no surface, and every surface is a pass-through onto it that only translates and presents; because two correct copies of the same operation pass every behavioral test, a structural test in the build holds that oneness.

**Version 0.1** is a draft with two rules. Parts of the scope are not yet written.

## Start here

The [concept](./concept.md) describes two implementations of the same operation that drift apart unnoticed, the load-bearing structure and the three-step procedure; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Two rules: F1 says that every operation exists once, F2 how that is held.

* [F1 One operation, two pass-throughs](./rules/F1-one-operation-two-pass-throughs.md)
* [F2 Oneness is checked structurally](./rules/F2-oneness-is-checked-structurally.md)

## Choices

This concept has no choices.

## Scope and evidence

The [scope](./scope.md) names security considerations, what it costs, the precondition and what is not yet settled; parts are not yet written. The [evidence](./evidence.md) gives the reference implementation and the incident of the ten drifted places; the [sources](./sources.md) name one recommended companion; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for builders; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
