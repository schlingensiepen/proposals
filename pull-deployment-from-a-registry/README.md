---
type: Proposal
description: In a pull deployment nobody stands at the target, so every assurance of the chain is placed either before publishing, in a build that runs the image against itself, computes its names and measures its visibility, or in what the operator holds, a deployment file whose header says what they pull and a call that tells them what is running.
version: "1.1"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - build-stamp
  - where-a-secret-comes-from
  - proven-from-outside
---

# Pull deployment from a registry

This directory is an OKF bundle for one proposal of the series: what a builder owes the operator when a service is built as an image, published to a registry and pulled by the operator from there, and where each check of that chain can be placed when nobody pushes anything onto the target machine.

**Question.** I build my service as an image, publish it to a registry, and the operator pulls it from there. Where can this chain break silently, and what do I owe the operator so that they can tell what is running on their side?

**Thesis.** In a pull deployment nobody stands at the target who could notice a fault. So every assurance of the chain is placed either before publishing, in a build that runs the image against itself, computes its names and measures its visibility, or in what the operator holds: a deployment file whose header says what they pull, and a call that tells them, after the rollout, what is running.

**Version 1.1** is a draft: the duties before publishing are built in a reference delivery and have caught their first incident; the handover to the operator is only partly met there, and all operators of the reference are the author. This version names what the reference has not built in the rules themselves, imports the proof of checks from `proven-from-outside`, and brings the wording of the derived files into line with the rules.

## Start here

The [concept](./concept.md) describes the chain that breaks silently, where every check is placed when nobody stands at the target, and the procedure for builder and operator; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Eight rules: the model F1, duties before publishing F2 to F5, handover to the operator F6 and F7, outer edge F8.

* [F1 In a pull, nobody stands at the target](./rules/F1-nobody-stands-at-the-target.md)
* [F2 The smoke-test checklist](./rules/F2-the-smoke-test-checklist.md)
* [F3 Names are computed, not formed](./rules/F3-names-are-computed-not-formed.md)
* [F4 latest is a promise](./rules/F4-latest-is-a-promise.md)
* [F5 Visibility is measured](./rules/F5-visibility-is-measured.md)
* [F6 The header of the deployment file](./rules/F6-the-header-of-the-deployment-file.md)
* [F7 Rolled out is not running](./rules/F7-rolled-out-is-not-running.md)
* [F8 What the outer edge advertises must get through](./rules/F8-what-the-outer-edge-advertises-must-get-through.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled; the [evidence](./evidence.md) shows the build pipeline and deployment files of the reference delivery with their incidents. The [sources](./sources.md) name the three imported concepts, the alternative and a related proposal; [used by](./used-by.md) lists which proposals import or extend this one. The [example](./examples/an-operator-pulls-starts-and-asks.md) follows an operator who pulls, starts and asks.

## Adopting

The [agent guide](./AGENT.md) is the guide for an agent acting as builder or operator, including the decisions it puts to the user; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
