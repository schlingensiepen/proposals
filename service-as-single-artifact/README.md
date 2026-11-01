---
type: Proposal
description: One service, one function, one artifact with several modes, one deliverable; configuring the function and issuing access are one act, so roles, rights management and admin never arise.
version: "1.2"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - one-configuration-one-judgment
  - where-a-secret-comes-from
  - maildir-style-file-queues
  - build-stamp
---

# The service as a single artifact

This directory is an OKF bundle for one proposal of the series: a procedure for whoever wants to put one function on the network and builds a small service for it. It is not an interface design pattern but the way to build one.

**Question.** I want to put a function on the network and build a small service for it. Which rules do I take as the foundation so that it stays small and is still safe to operate?

**Thesis.** A service carries exactly one function, is built as one artifact with several modes and delivered as one deliverable; the commands configure the function for the specific use case and create the access for exactly that configuration. Roles, rights management and admin therefore never arise.

**Version 1.2** adds one sentence to F4: the deliverable says which revision it was built from, after the imported concept `build-stamp`. Version 1.1 kept all eleven rules of 1.0 and added two: the boundary between configuration and state (F12) and data migration at start (F13); precedence, the empty value and the origin of start secrets moved into two imported concepts, and the choice of how commands enter the system became explicit (W1).

**The parts.**

* Start here: the [concept](./concept.md) (the problem, the concept with its load-bearing structure, the eleven-step procedure) and the [terms](./terms.md) in English and German.
* Rules, one answer each; cut: [F1 One service, one function](./rules/F1-one-service-one-function.md), [F2 One artifact, several modes](./rules/F2-one-artifact-several-modes.md), [F3 Commands are scriptable](./rules/F3-commands-are-scriptable.md), [F4 One delivery artifact, one delivery](./rules/F4-one-deliverable-one-delivery.md), [F5 One configuration, one judgment](./rules/F5-one-configuration-one-judgment.md), [F6 The state lies outside the artifact](./rules/F6-state-lies-outside-the-artifact.md); separation: [F7 The running service has no management surface](./rules/F7-no-management-surface-in-the-running-service.md), [F8 The permission cut runs between the modes](./rules/F8-the-permission-cut-runs-between-the-modes.md), [F9 Configuration and access come into being in one act](./rules/F9-configuration-and-access-in-one-act.md), [F10 The service knows no administrative rights](./rules/F10-no-administrative-rights.md); outer edge: [F11 No TLS in the service, local endpoint](./rules/F11-no-tls-in-the-service-local-endpoint.md); state: [F12 Configuration versus state](./rules/F12-configuration-versus-state.md), [F13 Data migration runs at start](./rules/F13-data-migration-runs-at-start.md).
* Choices, decided by whoever adopts the concept: [W1 Channel of the commands](./choices/W1-channel-of-the-commands.md).
* Scope and evidence: [scope](./scope.md) (security, cost, where it applies, when the answer flips, what it does not solve, what is not yet settled), [evidence](./evidence.md) (the draft-filing service from beginning to end), [sources](./sources.md) (imports, related proposals, external sources), [used by](./used-by.md), and the [example request](./examples/request-imap-sync.md) the building tests ran against.
* Adopting: the [agent guide](./AGENT.md) and the [extensions](./extensions.md).
* History: the [update log](./log.md).

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
