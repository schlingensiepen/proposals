---
type: Proposal
description: Each component is delivered by a versioned script steered by one directory per instance outside version control; the script creates and places the system's secrets itself, and because one part only gets its access while another holds a window open, the order of the partial deployments follows from the flow of secrets rather than from an agreement.
version: "1.0"
date: 2026-08-15
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# A push deployment that keeps its secrets

This directory is an OKF bundle for one proposal of the series: a procedure for whoever delivers a system of several parts by pushing it from their own machine to the hosts it runs on, and sets up its access control along the way.

**Question.** How do I deliver a system of several parts automatically, set up the access control along the way, and give away neither the accesses to the host systems nor the secrets that arise in the process?

**Thesis.** One versioned script per component, one directory per instance outside version control; the script owns the secrets and creates the accesses in a time-limited window, and from that, not from a convention, follows the order of the partial deployments.

**Version 1.0** is a draft: the procedure is fully decided and specified, but the scripts have not been built yet; the claims are thought through, not proven.

## Start here

The [concept](./concept.md) describes the two kinds of secrets, one script per component and one directory per instance, and the procedure in six steps; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Ten rules: separation and secrets F1 and F2, protection of the store F3 to F5, window and order F6 and F7, tooling F8 to F10.

* [F1 One script per component, one directory per instance](./rules/F1-one-script-per-component-one-directory-per-instance.md)
* [F2 The script owns the secrets of the system](./rules/F2-the-script-owns-the-system-secrets.md)
* [F3 What is delivered is a list from version control](./rules/F3-a-list-from-version-control-is-delivered.md)
* [F4 Runtime data is locked firmly, not exempted by configuration](./rules/F4-runtime-data-is-locked-not-exempted.md)
* [F5 The production configuration is created, never overwritten](./rules/F5-the-production-configuration-is-created-never-overwritten.md)
* [F6 Accesses for other components arise in a time-limited window](./rules/F6-accesses-for-other-components-arise-in-a-time-limited-window.md)
* [F7 The order follows the flow of secrets](./rules/F7-the-order-follows-the-flow-of-secrets.md)
* [F8 One entry point that runs, creates and lists](./rules/F8-one-entry-point-that-runs-creates-and-lists.md)
* [F9 Every script checks from outside what it has done](./rules/F9-every-script-checks-from-outside-what-it-has-done.md)
* [F10 Every kind of delivery comes with a guide for manual operation](./rules/F10-every-kind-of-delivery-comes-with-a-manual-guide.md)

## Choices

This concept has no choices; every entry is a rule.

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when the answer flips, what it does not solve and what is not yet settled; the [evidence](./evidence.md) follows an invented three-component example to a compromised machine and states that there is no reference implementation yet. The [sources](./sources.md) name the alternative and the related proposals; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) gives the steps with the duties of adoption; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
