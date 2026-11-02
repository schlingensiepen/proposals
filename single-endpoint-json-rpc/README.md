---
type: Proposal
description: The entire API is a single POST endpoint with a JSON-RPC 2.0 envelope; credentials go in headers, binary data stays outside.
version: "1.1"
date: 2026-11-01
status: draft
author: Jörn Schlingensiepen
extends: []
imports: []
---

# One endpoint, one verb, one envelope

This directory is an OKF bundle for one proposal of the series: how to talk to the server when all you have is cheap shared hosting, with one `POST` endpoint speaking JSON-RPC 2.0, credentials in headers, and an explicitly justified path for everything binary. It is a base concept.

**Question.** How do I talk to the server when all I have is cheap PHP hosting — no URL rewriting, no libraries, no framework?

**Thesis.** Every call is a POST to a single endpoint; method, parameters and errors live in the body, the credentials in headers — and binary data stays outside.

**Version 1.1** names the one exception to F7 in its title, the redemption secret presented once in a request body; states in F5 that protocol errors carry only what the standard defines; adds the directory-listing check to the agent guide; and corrects the upload address in the evidence. Wording smoothed throughout; the other rules are unchanged in content. **Version 1.0** in bundle form: the content of the first complete version, unchanged, cut into one file per rule.

## Start here

The [concept](./concept.md) describes an API on cheap shared hosting: one endpoint with one envelope and a separate path for bytes; the [terms](./terms.md) give every term of this bundle in English and German.

## Rules

Twelve rules: address and envelope F1 to F6, credentials F7, binary data F8, F9, F11 and F12, one origin F10.

* [F1 One address, reached through the default document](./rules/F1-one-address-through-the-default-document.md)
* [F2 An established envelope, not a homegrown one](./rules/F2-an-established-envelope.md)
* [F3 No batch requests, no notifications](./rules/F3-no-batch-requests-no-notifications.md)
* [F4 The HTTP status is 200; the outcome lives in the envelope](./rules/F4-the-http-status-is-200.md)
* [F5 The error has three fields with three jobs](./rules/F5-the-error-has-three-fields.md)
* [F6 Protocol errors and application errors are separate number spaces](./rules/F6-protocol-and-application-errors-are-separate.md)
* [F7 Credentials go in headers, never in the body, never in the address, with one named exception](./rules/F7-credentials-go-in-headers.md)
* [F8 Binary data leaves the envelope, in both directions](./rules/F8-binary-data-leaves-the-envelope.md)
* [F9 File intake carries no application logic](./rules/F9-file-intake-carries-no-application-logic.md)
* [F10 Static delivery and API are one origin to the outside](./rules/F10-static-delivery-and-api-are-one-origin.md)
* [F11 The randomness is in the directory, not in the filename](./rules/F11-the-randomness-sits-in-the-directory.md)
* [F12 Large compilations: commission, poll, fetch, terminate](./rules/F12-large-compilations.md)

## Scope and evidence

The [scope](./scope.md) names security considerations, costs, preconditions, when this no longer holds, what it does not solve and what is not yet settled; the [evidence](./evidence.md) follows a complete operation and states that the concept is thought through, not yet proven. The [sources](./sources.md) name one related proposal, one tension and external sources; [used by](./used-by.md) lists which proposals import or extend this one.

## Adopting

The [agent guide](./AGENT.md) is the self-contained guide for building or reviewing an API on this concept; [extensions](./extensions.md) lists the proposals that extend this one.

## History

The [update log](./log.md) records the dated changes to this bundle.

**Reader:** the sections above tell you where to start. **Agent:** read `index.md`; it lists every file with one line so you can decide what to load. To adopt the concept, go to `AGENT.md`.

---

Part of the proposal series `schlingensiepen/proposals`. Licensed CC BY 4.0; the license text and the register of all proposals are in the root README of the repository.
