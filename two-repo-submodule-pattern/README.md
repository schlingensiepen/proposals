---
type: Proposal
description: What arises while something is being made and what is finally published belong in two repositories with two histories, connected as a submodule or, where the tools do not allow that, as two independent roots with an ignore rule.
version: "0.12"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - proven-from-outside
---

# Work repository and product repository

This directory is an OKF bundle for one proposal of the series: how to keep what the public is meant to see apart from what arises while it is being made, and how to connect the two without getting two truths.

**Question.** How do I separate what the public is meant to see from what arises while it is being made, without ending up with two truths and without losing the history of how it came about?

**Thesis.** Two repositories: one for the work with its whole history, one for the product with a history worth showing; connected either as a submodule, where the work repository embeds the product repository, or as two independent roots with an ignore rule, where the available tools cannot write submodule references.

**Version 0.12** is a draft: complete in content, eight rules, not yet reviewed by the author. What is not yet proven and not yet settled is listed in the scope.

## The parts

**Start here.** [`concept.md`](./concept.md) gives the problem, the concept with its two forms of embedding, the load-bearing structure and the usual allocation of workshop and product as a starting point; [`terms.md`](./terms.md) lists the terms in English and German.

**Rules.** Eight. [F1](./rules/F1-two-repositories-two-histories.md) is the separation itself, [F2](./rules/F2-the-work-embeds-the-product.md) its direction, [F3](./rules/F3-the-handover-is-deliberate-and-copies.md) the handover that copies and generates anew. [F4](./rules/F4-submodule-form.md) and [F5](./rules/F5-two-roots-form.md) are the two forms, decided by the tool situation. [F6](./rules/F6-writing-into-another-repository-only-with-announcement.md) is the announcement rule, [F7](./rules/F7-a-review-is-offered-before-every-handover.md) the review before every handover, [F8](./rules/F8-every-release-names-its-state-and-purpose.md) the release with state and purpose.

**Scope and evidence.** [`scope.md`](./scope.md) covers security, cost, where the concept applies and what is not yet settled; [`evidence.md`](./evidence.md) where it has held; [`sources.md`](./sources.md) what it imports and relates to; [`used-by.md`](./used-by.md) which proposals build on it.

**Adopting.** [`AGENT.md`](./AGENT.md) is the self-contained guide for an agent setting up the two repositories; [`extensions.md`](./extensions.md) lists extensions.

**History.** [`log.md`](./log.md) records dated changes.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
