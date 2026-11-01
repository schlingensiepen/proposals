---
type: Proposal
description: The state of an item is never changed but replaced; every change creates a new, complete state, and the history is the sequence of these states.
version: "0.2"
date: 2026-08-15
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# Nothing is overwritten

This directory is an OKF bundle for one proposal of the series: how to keep the history of an item so that it cannot be changed afterwards, without every change duplicating everything. **It is a fragment, deliberately unfinished.** The rules hold for flat items, where they were developed and tried out; for items that are themselves trees, this bundle is not an answer but an open question with some groundwork (see the scope).

**Question.** How do I record the history of an item so that it cannot be changed afterwards, and without every change duplicating everything?

**Thesis.** Nothing is overwritten: every change creates a new state, attachments are carried over by reference instead of copied, and exactly one metadata entry may still live on the current latest state.

**Version 0.2** is a fragment. It records what is decided so that it is not lost, and names the one question it hinges on: how the procedure carries over to object trees. Until that is settled, it will not be raised to 1.0.

## Start here

The [concept](./concept.md) describes the problem of losing nothing and duplicating nothing and the load-bearing structure; the procedure is not yet written. The [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Four rules, developed on a flat model; how they carry over to object trees is not yet settled.

* [F1 Every change creates a state](./rules/F1-every-change-creates-a-state.md)
* [F2 Attachments are carried over, not copied](./rules/F2-attachments-are-carried-over-not-copied.md)
* [F3 Exactly one living metadata entry, and only on the latest state](./rules/F3-exactly-one-living-metadata-entry.md)
* [F4 Creator and origin are snapshots, not references](./rules/F4-creator-and-origin-are-snapshots.md)

## Scope and evidence

The [scope](./scope.md) carries the fragment notice, the security points still to be worked out, the known costs and the open question of object trees; the [evidence](./evidence.md) is not yet written. The [sources](./sources.md) name one related proposal and one related external approach; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) states, in this fragment, only the purpose and its limit; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
