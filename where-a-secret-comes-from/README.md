---
type: Proposal
description: One ranking of sources for every start secret and every role, environment first, then a pass-through file, last a channel used exactly once, where the higher rank wins but never hides a lower one, and replacing is two moves.
version: "1.0"
date: 2026-10-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# Where a secret comes from

This directory is an OKF bundle for one proposal of the series: where a
service, a script or an agent takes the secret it needs before it can do
anything, in which order, and what it owes the person who set it. It is
a base concept that other proposals import.

**Question.** A service, a script or an agent needs a secret before it
can do anything. Where does it take it from, in which order, and what
does having an order oblige it to do for the person who set the secret?

**Thesis.** There is one ranking of sources, the same for every start
secret and every role: the environment, then a file that only passes
through, last a channel used exactly once. The higher rank wins but never
hides: lower ranks that are also set are reported, and replacing is two
moves. The once-only channel is the one named exception to the rule that
a secret never appears in any output, which is why it is narrow: one
value, once, at creation.

**Scope.** This concept governs the **start secret**: the one thing
that has to come from outside because something has to come first. Everything that arises later in
operation (accounts, accesses, passwords) the service keeps itself in
its own state, as a salted hash where it only verifies, in plain text
under tight permissions only where it must present it to a target
system. That is governed by the configuration-versus-state boundary of
`service-as-single-artifact`, not by this ranking.

Start with [`index.md`](./index.md); it lists every file with one line
each. Builders adopting the concept go to [`AGENT.md`](./AGENT.md).

---

Part of the proposal series `schlingensiepen/proposals`. Licensed
CC BY 4.0; the licence text and the register of all proposals are in the
root README of the repository.
