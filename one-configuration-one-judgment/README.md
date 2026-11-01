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

## What is in this bundle

**Start here:** [README](./README.md) — Identity card: question, thesis, version, what it imports. [Concept](./concept.md) — The problem, the concept in prose, the five-step procedure. [Terms](./terms.md) — The terms of this bundle in English and German, each with a short explanation.

**Rules** (one answer each; give one up and you are no longer using the concept): [F1 The judgment is the start](./rules/F1-the-judgment-is-the-start.md), [F2 The check names the source](./rules/F2-the-check-names-the-source.md), [F3 Secrets are referenced, not contained](./rules/F3-secrets-are-referenced-not-contained.md), [F4 Check without binding](./rules/F4-check-without-binding.md), [F5 Duplicate setting is reported](./rules/F5-duplicate-setting-is-reported.md).

**Choices** (more than one legitimate answer; you decide and record): [W1 Precedence: file or value](./choices/W1-precedence-file-or-value.md), [W2 The empty value](./choices/W2-the-empty-value.md).

**Scope and evidence:** [Scope](./scope.md) — Security considerations, what it costs, where it applies, where the answer flips, what is not yet proven. [Evidence](./evidence.md) — Where it has held: origin in the series, the empty-value incident, the second truth. [Sources](./sources.md) — What this proposal builds on: the concept it imports, related proposals, external sources. [Used by](./used-by.md) — Which proposals import or extend this one. Only needed when changing this concept.

**Adopting:** [AGENT](./AGENT.md) — Self-contained guide for an agent adopting this concept, including the mandatory step of deciding the choices with the user. [Extensions](./extensions.md) — Proposals that extend this one. None yet.

**History:** [Update log](./log.md) — Dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed
CC BY 4.0; the license text and the register of all proposals are in the
root README of the repository.
