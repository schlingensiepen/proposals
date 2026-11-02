---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, version.
* [Concept](./concept.md) - The problem, the concept in prose with its load-bearing structure, the procedures it introduces.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. Humans F1 to F5, the boundary to machines F6 to F8, transport F9, rights F10.

* [F1 There are no guests](./rules/F1-there-are-no-guests.md) - Every access to content requires a credential, reading included; possession of a hard-to-guess address never suffices.
* [F2 Access arises from an invitation, not from registration](./rules/F2-access-arises-from-an-invitation.md) - No sign-in form, no password, no self-registration; two procedures, admitting somebody new and re-inviting an existing user.
* [F3 The invitation token is not the credential](./rules/F3-the-invitation-token-is-not-the-credential.md) - The token permits only being exchanged, any number of times; every device records its invitation; withdrawal and an expiry are the levers.
* [F4 One credential per device](./rules/F4-one-credential-per-device.md) - The record is "user on device"; the credential lives in a server-set cookie; "device" means browser profile.
* [F5 Authorization runs against the store](./rules/F5-authorization-against-the-store.md) - The credential is an opaque reference stored as a hash; rights are looked up in four checks of fixed order on every access.
* [F6 Humans receive identity, machines receive capability](./rules/F6-humans-identity-machines-capability.md) - A machine gets a key for one task at one place, representing nobody, one per task and per machine.
* [F7 Machine accesses come into being only in the bootstrap window](./rules/F7-machine-keys-only-in-the-bootstrap-window.md) - Creating machine keys is bound to the window and shows the clear text once; revoking is always available.
* [F8 The bootstrap is a time window, not a delete-me file](./rules/F8-the-bootstrap-is-a-time-window.md) - Setup procedures require the strongest secret outside the store and a window a deployment opens.
* [F9 Secrets in addresses go in the fragment, or nowhere](./rules/F9-secrets-in-addresses-sit-in-the-fragment.md) - Never in path or query string; read from the fragment and sent once in a request body.
* [F10 The model provides two anchors](./rules/F10-two-anchors-for-a-rights-system.md) - No rights model; user and user on device as anchors, and five properties demanded of any rights system built on them.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when this no longer holds, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - A worked example in an invented inventory system, the missing reference implementation, and the origin.
* [Sources](./sources.md) - Related proposals and external sources (cookie and URI specifications, session management guidance).
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent adopting this concept: rules, the deadline decided with the user, procedures including the setup of a fresh system, acceptance checklist, common mistakes, self-check.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
