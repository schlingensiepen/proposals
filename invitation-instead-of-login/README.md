---
type: Proposal
description: Access arises from an invitation; one link invites a person for all their devices because every redemption creates its own credential, and machines receive capability rather than identity.
version: "1.0"
date: 2026-08-15
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# Invited, not logged in

This directory is an OKF bundle for one proposal of the series: an access model for an application that serves a small, known circle, with no login form, no password and no guests, and with a clear line between the access of humans and the access of machines.

**Question.** How does the server know who is calling, without a login form, without a password, and without guests?

**Thesis.** Access arises from an invitation; one link invites a user for all their devices, because every redemption creates its own credential, and machines receive capability rather than identity.

**Version 1.0** in bundle form: the content of the first complete version, arranged as one file per rule, with the open questions of that version turned into duties of adoption and into what the scope names as not yet settled.

## Start here

The [concept](./concept.md) describes the problem, the access model with its load-bearing structure and the procedures it introduces; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Ten rules: humans F1 to F5, the boundary to machines F6 to F8, transport F9, rights F10.

* [F1 There are no guests](./rules/F1-there-are-no-guests.md)
* [F2 Access arises from an invitation, not from registration](./rules/F2-access-arises-from-an-invitation.md)
* [F3 The invitation token is not the credential](./rules/F3-the-invitation-token-is-not-the-credential.md)
* [F4 One credential per device](./rules/F4-one-credential-per-device.md)
* [F5 Authorization runs against the store](./rules/F5-authorization-against-the-store.md)
* [F6 Humans receive identity, machines receive capability](./rules/F6-humans-identity-machines-capability.md)
* [F7 Machine accesses come into being only in the bootstrap window](./rules/F7-machine-keys-only-in-the-bootstrap-window.md)
* [F8 The bootstrap is a time window, not a delete-me file](./rules/F8-the-bootstrap-is-a-time-window.md)
* [F9 Secrets in addresses sit in the fragment, or nowhere](./rules/F9-secrets-in-addresses-sit-in-the-fragment.md)
* [F10 The model provides two anchors](./rules/F10-two-anchors-for-a-rights-system.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, where it applies, when the answer flips, what it does not solve and what is not yet settled; the [evidence](./evidence.md) gives a worked example in an invented inventory system and the origin. The [sources](./sources.md) name related proposals and external specifications; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for building or reviewing access control of this kind; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
