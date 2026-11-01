---
type: Proposal Extension
description: Where the operator has no shell, a service built as one artifact gets a complete second administration surface over HTTP beside its commands, on its own port, off as shipped, with a root secret it generates and shows once, and both surfaces pass through to one operation.
version: "1.0"
date: 2026-10-13
status: draft
author: Jörn Schlingensiepen
extends:
  - service-as-single-artifact
imports:
  - where-a-secret-comes-from
  - service-as-a-container
  - one-operation-many-surfaces
offer: If you must operate the service without a shell on its host, extend it with an administration surface over HTTP
---

# The administration surface beside the command line

This directory is an OKF bundle for one proposal of the series: an extension for whoever runs a service built as one artifact with command-line administration in a place where the operator has no shell and no one-off invocation against the running container. It extends `service-as-single-artifact` and is the HTTP form of that concept's choice W1 (b), commands brought into the system over another channel. It lifts F7, F10 and part of F8 of `service-as-single-artifact`: the running service gets an administration surface, an administration secret with power over accesses, and a service process that writes the store.

**Question.** I run my service through a container console and have no shell on the host system. I can start it, but nobody can issue the first access. How do I make it operable without losing the cut that gets by without an administration surface and without administrative rights?

**Thesis.** Whoever has to run a service where there is no shell builds, beside the command, a complete second administration surface over HTTP, on its own port, switched off as shipped, with an administration secret the service generates on first start and writes into its log exactly once, and keeps both surfaces pass-throughs onto one operation that exists exactly once and is checked structurally for it; the price is the root of a rights hierarchy the cut otherwise does not know, and it is named rather than softened.

## Start here

The [concept](./concept.md) describes the service that starts but cannot be put into operation without a shell, the load-bearing structure and the thirteen-step procedure; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Twelve rules: cut F1 to F3, place F4 and F5, administration secret F6 to F9, limits F10 to F12.

* [F1 Only out of need, then complete, beside the command](./rules/F1-only-out-of-need-then-complete-beside-the-command.md)
* [F2 One operation, two pass-throughs](./rules/F2-one-operation-two-pass-throughs.md)
* [F3 Oneness is checked structurally](./rules/F3-oneness-is-checked-structurally.md)
* [F4 Own port, own application, own name](./rules/F4-own-port-own-application-own-name.md)
* [F5 Switched off as shipped](./rules/F5-off-as-shipped.md)
* [F6 The service generates the administration secret](./rules/F6-the-service-generates-the-administration-secret.md)
* [F7 No silent fallback](./rules/F7-no-silent-fallback.md)
* [F8 The ranking and the latent access](./rules/F8-the-ranking-and-the-latent-access.md)
* [F9 A root, not a leaf](./rules/F9-a-root-not-a-leaf.md)
* [F10 No secrets out, no general update](./rules/F10-no-secrets-out-no-general-update.md)
* [F11 The surface explains itself](./rules/F11-the-surface-explains-itself.md)
* [F12 The permission cut returns](./rules/F12-the-permission-cut-returns.md)

## Choices

This extension has no choices of its own; adopting it decides the extended concept's choice on the channel of the commands, and the choices it decides of the imported `where-a-secret-comes-from` are named in the rules and the agent guide.

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when the answer flips, what it does not solve and what is not yet settled; the [evidence](./evidence.md) shows the reference implementation, what it evidences, what is only partly met, and the incidents. The [sources](./sources.md) name the concept this extension extends, the concepts it imports and related proposals; [used by](./used-by.md) lists which proposals import or extend this one. The [example](./examples/commissioning-without-a-shell.md) walks through taking the service into operation without a shell.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for builders, operators and administering agents; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
