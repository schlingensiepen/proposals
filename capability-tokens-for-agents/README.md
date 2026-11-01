---
type: Proposal
description: A token embodies exactly one capability at exactly one place, and its handling follows fixed rules along the lifecycle, from creation and handover through storage and use to rotation, revocation and expiry, so that an agent context in which every leak is searchable and copyable survives partial failure.
version: "1.1"
date: 2026-10-13
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - where-a-secret-comes-from
---

# Capability tokens for agents

This directory is an OKF bundle for one proposal of the series: how to
design, hand over, store and use access tokens when the consumer of a
service is an AI agent. It applies to every service that agents
consume, whether web API, command line or MCP, self-describing or not.

**Question.** How does one design, hand over, store, and use access
tokens when the consumer of a service is an AI agent?

**Thesis.** A token embodies exactly one capability at exactly one
place, and its handling follows fixed rules along the lifecycle,
because in the agent context every leak is searchable and copyable;
whoever pins down both the cut and the handling survives partial
failure gracefully.

**In short.** When the consumer of a service is an AI agent, the leak
channels for credentials change fundamentally: the chat history
becomes a searchable, copyable storage medium, and the agent can be
steered by foreign text. The most effective answer is a double cut:
**tokens as narrow capabilities** on the service side and **fixed
handling rules** on the agent side.

**Version 1.1** keeps all twelve rules of 1.0. The ranking by which
an agent obtains and keeps a secret (F6) is now imported from
`where-a-secret-comes-from`; F6 keeps the agent's part in it.

## Start here

The [concept](./concept.md) describes the problem and the double cut with its map, its load-bearing structure and the procedure for operator and agent; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Twelve rules: cut on the service side F1 to F4, lifecycle F5 to F9, agent behavior and its delivery F10 to F12.

* [F1 One token, one ability, one place](./rules/F1-one-token-one-ability-one-place.md)
* [F2 Creation: server-side, high-entropy, visible only once](./rules/F2-creation-server-side-high-entropy-visible-once.md)
* [F3 The token is the scope](./rules/F3-the-token-is-the-scope.md)
* [F4 Transport and error semantics: no oracles, no leak surfaces](./rules/F4-transport-and-error-semantics.md)
* [F5 Handover over a second channel, recommended: self-registration](./rules/F5-handover-over-a-second-channel.md)
* [F6 Storage in a fixed ranked order](./rules/F6-storage-in-a-fixed-ranked-order.md)
* [F7 Binding to the place of issue](./rules/F7-binding-to-the-place-of-issue.md)
* [F8 Rotation and revocation hit exactly one purpose](./rules/F8-rotation-and-revocation-hit-one-purpose.md)
* [F9 Expiry is the operator's duty, the values are set by the adopter](./rules/F9-expiry-is-the-operators-duty.md)
* [F10 The agent's never-rules](./rules/F10-the-agents-never-rules.md)
* [F11 Way of speaking: the display string](./rules/F11-way-of-speaking-the-display-string.md)
* [F12 Delivery duty: the code travels with the service](./rules/F12-the-code-travels-with-the-service.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when the answer flips, what it does not solve and what is not yet settled; the [evidence](./evidence.md) gives the origin, the worked application, the precursor in operation and a fictitious walk-through. The [sources](./sources.md) name the imported concept, a related proposal and external sources; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for operators and agents, including the values to settle with the user; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed
CC BY 4.0; the license text and the register of all proposals are in
the root README of the repository.
