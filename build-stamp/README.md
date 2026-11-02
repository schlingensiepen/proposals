---
type: Proposal
description: The build stamps the source revision into the artifact itself, the artifact reports it when asked against the running instance, and the build pipeline publishes nothing that cannot report exactly the revision it was built from.
version: "0.2"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# The build stamp

This directory is an OKF bundle for one proposal of the series: how an artifact says which revision of its source it was built from, so that statements about what is running can be checked. It is a base concept that holds for every artifact; other proposals import it.

**Question.** I have built, published and deployed an artifact. How do I tell which revision of the source the thing that is running right now was built from, and how do I keep from talking about something other than what is running?

**Thesis.** The build stamps the source revision into the artifact itself, the artifact reports it when asked against the running instance, and the build pipeline publishes only a deliverable that reports exactly the revision built before it is published; without the stamp, every statement about the behavior of what is running is unverifiable.

**Version 0.2** is a draft with one rule. Parts of the procedure, the scope and the evidence are not yet written.

## Start here

The [concept](./concept.md) describes statements about the wrong revision, the stamp that travels inside the artifact, and a short procedure; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

One rule.

* [F1 The artifact says which revision it was built from](./rules/F1-the-artifact-says-what-it-was-built-from.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies and what is not yet settled; the [evidence](./evidence.md) tells of the container that ran seven changes behind and of the stamp and smoke test since. The [sources](./sources.md) name no imports and no external sources yet; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) gives three steps, the rule, an acceptance checklist and common mistakes; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
