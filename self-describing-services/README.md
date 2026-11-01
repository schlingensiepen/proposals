---
type: Proposal
description: A service that describes itself at runtime, in staged and linked texts its weakest intended reader can accept, makes installation unnecessary; the agent keeps only a URL and a secret, and because there is no second source, nothing goes stale.
version: "1.2"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - where-a-secret-comes-from
  - proven-from-outside
---

# Self-describing services

This directory is an OKF bundle for one proposal of the series: how a web service describes itself to agents so that they can use it without installing anything, on every platform and in every sandbox. It addresses whoever builds such a service, whoever retrofits a foreign API, and, through the service's own description, the agent that uses it.

**Question.** How, as a user, do I make a service available to my agents, without installation, on every platform, in every sandbox?

**Thesis.** A service that describes itself makes installation unnecessary: the agent remembers only the classic tool description ("if you want this, look at this URL") and a token; everything else the service delivers itself at runtime, and because there is no second source, nothing goes stale.

**Version 1.2** corrects three example hostnames and the sentence on choices; nothing else changed. **Version 1.1** keeps the fifteen rules of 1.0 and adds seven: five on arriving at the reader (F16 to F20: delivery form, present tense, closure, one word for one thing, and texts that are cut with an entry page that links informatively), one on what the entry page answers before the service is set up (F21), and one on stating every behavior rule exactly once, held by a check in the build (F22). The ranking of secret sources is now imported (F13), and so are the proofs that a check tests anything and that a description works for a reader without inside knowledge.

## The parts

**Start here.** [`concept.md`](./concept.md) gives the problem, the concept with its load-bearing structure and the two procedures, one for the builder and one for the consumer; [`terms.md`](./terms.md) lists the terms in English and German.

**Rules.** Twenty-two, each with one answer, in six groups: building the description (F1 to F7), the consumer side (F8 to F11), the access gear (F12 to F14), retrofit (F15), arriving at the reader (F16 to F20), and before setup and holding the separation (F21, F22). Each is a file under [`rules/`](./rules/); [`index.md`](./index.md) lists them with one line each.

**Scope and evidence.** [`scope.md`](./scope.md) covers security, what each rule costs, where the concept applies, when the answer flips, what it does not solve and what is not yet settled; [`evidence.md`](./evidence.md) where it has held; the invented [traffic service](./examples/traffic-service-walkthrough.md) plays the whole flow through; [`sources.md`](./sources.md) says what the concept builds on; [`used-by.md`](./used-by.md) which proposals import it.

**Adopting.** [`AGENT.md`](./AGENT.md) is the self-contained guide for builder and consumer; [`extensions.md`](./extensions.md) lists extensions.

**History.** [`log.md`](./log.md) records dated changes.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
