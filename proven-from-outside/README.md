---
type: Proposal
description: A check proves something only once it has been seen to go red with its safeguard removed, and a document meant to work without inside knowledge counts as accepted only once a reader without that knowledge has completed a task with it.
version: "0.1"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# Proven from outside

This directory is an OKF bundle for one proposal of the series: how you
know that a check actually checks something, and that a document meant to
work without you actually works, when you read both with the knowledge
they are meant to replace. It is a small base concept that holds for every
check and every such document; other proposals import it.

**Question.** My checks are green, and my document is meant to work
without me. How do I tell that the checks test anything at all and that
the document is enough, when I read both with the very knowledge they are
meant to replace?

**Thesis.** Proof comes from outside: a check proves something only once
it has been seen to go red with its safeguard removed, and a document
meant to work without inside knowledge is accepted only once a reader
without that knowledge has completed a task with it; whoever checks what
they built themselves shares the blind spots of what is under review.

**Version 0.1** is a draft with two rules. Parts of the procedure and the
scope are not yet written.

## Start here

The [concept](./concept.md) describes checks and documents judged from the inside, the concept of proof from outside, and a two-step procedure; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Two rules, each standing on its own. There are no choices.

* [F1 A check that cannot go red is no check](./rules/F1-a-check-that-cannot-go-red-is-none.md)
* [F2 Acceptance by a reader without inside knowledge](./rules/F2-acceptance-by-a-reader-without-inside-knowledge.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, what it costs, where it applies and what is not yet settled; the [evidence](./evidence.md) tells of three green checks that tested nothing and of nine rounds of self-review against the first outside run. The [sources](./sources.md) name one proposal recommended with this one; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) gives two steps, the rules, an acceptance checklist and common mistakes; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed
CC BY 4.0; the license text and the register of all proposals are in the
root README of the repository.
