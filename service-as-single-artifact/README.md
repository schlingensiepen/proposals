---
type: Proposal
description: One service, one function, one artifact with several modes, one deliverable; configuring the function and issuing access are one act, so roles, rights management and admin never arise.
version: "1.1"
date: 2026-10-11
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - one-configuration-one-judgment
  - where-a-secret-comes-from
  - maildir-style-file-queues
---

# The service as a single artifact

This directory is an OKF bundle for one proposal of the series: a procedure for whoever wants to put one function on the network and builds a small service for it. It is not an interface design pattern but the way to build one.

**Question.** I want to put a function on the network and build a small service for it. Which rules do I take as the foundation so that it stays small and is still safe to operate?

**Thesis.** A service carries exactly one function, is built as one artifact with several modes and delivered as one deliverable; the commands configure the function for the specific use case and create the access for exactly that configuration. Roles, rights management and admin therefore never arise.

**Version 1.1** keeps all eleven rules of 1.0 and adds two: the boundary between configuration and state (F12) and data migration at start (F13). Precedence, the empty value and the origin of start secrets have moved into two imported concepts; the choice of how commands enter the system is now explicit (W1).

Start with [`index.md`](./index.md); it lists every file with one line each. Builders adopting the concept go to [`AGENT.md`](./AGENT.md).

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
