---
type: Proposal
description: One ranking of sources for every start secret and every role, environment first, then a pass-through file, last a channel used exactly once, where the higher rank wins but never hides a lower one, and replacing is two moves.
version: "1.2"
date: 2026-11-01
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

## What is in this bundle

**Start here:** [README](./README.md) — Identity card: question, thesis, scope, version. [Concept](./concept.md) — The problem, the concept in prose, the six-step procedure. [Terms](./terms.md) — The terms of this bundle in English and German, each with a short explanation.

**Rules** (one answer each; give one up and you are no longer using the concept): [F1 The ranking is fixed and named](./rules/F1-the-ranking-is-fixed-and-named.md), [F2 Environment first, under a canonical name](./rules/F2-environment-first-under-a-canonical-name.md), [F3 The file only passes through](./rules/F3-the-file-only-passes-through.md), [F4 The higher rank wins and never hides](./rules/F4-the-higher-rank-wins-and-never-hides.md), [F5 Replacing is two moves](./rules/F5-replacing-is-two-moves.md), [F6 Never in any output](./rules/F6-never-in-any-output.md).

**Choices** (more than one legitimate answer; you decide and record): [W1 No secret set](./choices/W1-no-secret-set.md), [W2 Set but empty or unusable](./choices/W2-set-but-empty-or-unusable.md), [W3 Further sources](./choices/W3-further-sources.md).

**Scope and evidence:** [Scope](./scope.md) — Security considerations, what it costs, where it applies, when this no longer holds, what it does not solve, what is not yet proven. [Evidence](./evidence.md) — Where it has held: origin in the series, the latent access incident, the named exception. [Sources](./sources.md) — What this proposal builds on: related proposals, a tension, external sources. [Used by](./used-by.md) — Which proposals import or extend this one. Only needed when changing this concept.

**Adopting:** [AGENT](./AGENT.md) — Self-contained guide for an agent adopting this concept, including the mandatory step of deciding the choices with the user. [Extensions](./extensions.md) — Proposals that extend this one. None yet.

**History:** [Update log](./log.md) — Dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed
CC BY 4.0; the license text and the register of all proposals are in the
root README of the repository.
