---
type: Proposal
description: One configuration, judged once by the code that will run on it; every value names where it came from, and an empty or unusable value is a reported error, never a silent default.
version: "1.0"
date: 2026-10-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports:
  - where-a-secret-comes-from
---

# One configuration, one judgment

This directory is an OKF bundle for one proposal of the series: how a
service judges its configuration before it binds anything, and how it
tells the operator which values it actually took. It is a base concept
that other proposals import; it says nothing about what the service does.

**Question.** A service reads its operating parameters from several
places: the command line, the environment, a file near the user, a file
in the system. How does it reach one verdict before the first bind, a
verdict the start will not revoke, and how does the operator know which
value the service actually used?

**Thesis.** There is exactly one judgment about the configuration, and
it is made by the code that starts on it. Every value has a place in a
fixed precedence and names its source, and a value that is set but
empty or unusable is an error of whoever set it, not an occasion for a
default.

Start with [`index.md`](./index.md); it lists every file with one line
each. Builders adopting the concept go to [`AGENT.md`](./AGENT.md).

---

Part of the proposal series `schlingensiepen/proposals`. Licensed
CC BY 4.0; the licence text and the register of all proposals are in the
root README of the repository.
