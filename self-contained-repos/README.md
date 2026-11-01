---
type: Proposal
description: A repository is self-contained when someone opening it for the first time, human or agent, can work from it alone; an entry point that branches to the actual tasks, decisions with their reasons in full text, and a check rule asking whether everything still holds when every external link is dead.
version: "0.9"
date: 2026-08-15
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - proven-from-outside
---

# The self-contained repository

This directory is an OKF bundle for one proposal of the series: how to shape a repository so that whoever opens it for the first time, a human or an agent, can understand it and contribute to it from the repository alone. It is a base concept.

**Question.** How do I shape a repository so that a human or an agent who opens it for the first time can work from it alone, without prior knowledge, without asking back, without outside documents?

**Thesis.** A repository carries within itself everything needed to understand it and contribute to it: an entry point that branches to the two or three actual tasks, decisions together with their reasons in full text instead of as a link, and a check rule for it: does it still work when every external link is dead?

**Version 0.9** is a working version: complete in content, seven rules, not yet reviewed by the author. What is not yet settled or not yet proven is stated in the scope.

## The parts

**Start here.** [`concept.md`](./concept.md) gives the problem, the concept with its three components and its load-bearing structure, and the order in which the rules are applied; [`terms.md`](./terms.md) lists the terms in English and German.

**Rules.** Seven. [F1](./rules/F1-the-dead-link-test.md) is the check rule, does it work with dead links? [F2](./rules/F2-an-entry-point-that-branches.md) to [F4](./rules/F4-instructions-are-self-contained.md) concern the structure: an entry point that branches, one task definition per recurring activity, instructions that are self-contained. [F5](./rules/F5-decisions-come-with-their-reasons.md) brings decisions in with their reasons. [F6](./rules/F6-redundancy-is-maintained.md) and [F7](./rules/F7-generated-is-marked-as-generated.md) concern maintenance: redundancy maintained, generated files marked. There are no choices.

**Scope and evidence.** [`scope.md`](./scope.md) covers security, cost, where the concept applies and what is not yet settled; [`evidence.md`](./evidence.md) where it has held; [`sources.md`](./sources.md) what it imports and relates to; [`used-by.md`](./used-by.md) which proposals build on it.

**Adopting.** [`AGENT.md`](./AGENT.md) is the self-contained guide for an agent that makes a repository self-contained or reviews one; [`extensions.md`](./extensions.md) lists extensions.

**History.** [`log.md`](./log.md) records dated changes.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
