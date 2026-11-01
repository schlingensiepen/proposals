---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: what this extension extends and lifts, question, thesis, what it imports.
* [Concept](./concept.md) - The problem of the service that starts but cannot be put into operation without a shell, the concept with its load-bearing structure, the thirteen-step procedure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the extension. Cut F1 to F3, place F4 to F5, administration secret F6 to F9, limits F10 to F12.

* [F1 Only out of need, then complete, beside the command](./rules/F1-only-out-of-need-then-complete-beside-the-command.md) - Only where the operator has neither shell nor one-off invocation; then every administrative action, store-wide ones included; the command stays the normal way.
* [F2 One operation, two pass-throughs](./rules/F2-one-operation-two-pass-throughs.md) - Command line and administration surface as pass-throughs onto one operation under the imported one-operation-many-surfaces; here neither surface calls the other, and every route mirrors a command.
* [F3 Oneness is checked structurally](./rules/F3-oneness-is-checked-structurally.md) - The structural test of the imported one-operation-many-surfaces, run over both pass-throughs with the same list of operation names.
* [F4 Own port, own application, own name](./rules/F4-own-port-own-application-own-name.md) - No path prefix; no shared login; absent from the user-facing description; an unbindable port aborts the start.
* [F5 Switched off as shipped](./rules/F5-off-as-shipped.md) - An empty address means no port, no secret, no generation; the delivery form addressed to the operator without a shell switches it on.
* [F6 The service generates the administration secret](./rules/F6-the-service-generates-the-administration-secret.md) - A start secret under the imported where-a-secret-comes-from; here a root, generated and written into the log exactly once.
* [F7 No silent fallback](./rules/F7-no-silent-fallback.md) - A set but unusable secret, an unwritable or unreadable file and an unbindable port abort the start.
* [F8 The ranking and the latent access](./rules/F8-the-ranking-and-the-latent-access.md) - Lower ranks reported, change in two moves per the import; said on the entry page and in the deployment file.
* [F9 A root, not a leaf](./rules/F9-a-root-not-a-leaf.md) - Revocation, change, lifetime and loss of the administration secret; treated like an administrative password.
* [F10 No secrets out, no general update](./rules/F10-no-secrets-out-no-general-update.md) - Output only at creation, with the address; changing a variant is removing and creating anew.
* [F11 The surface explains itself](./rules/F11-the-surface-explains-itself.md) - Entry page, refusals, format statement, readiness point; one check rule for both surfaces.
* [F12 The permission cut returns](./rules/F12-the-permission-cut-returns.md) - Two processes with two users, or the waiver as a sentence in the security considerations.

# Choices

This extension has no choices of its own. Adopting it decides the extended concept's choice on the channel of the commands; the choices it decides of the imported where-a-secret-comes-from are named in the rules and the agent guide.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when the answer flips, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - Where it has held: the reference implementation with what it evidences, what is only partly met or not built, the incidents, and the origin.
* [Sources](./sources.md) - What this extension builds on: the concept it extends, the concepts it imports, related proposals, the reference implementation.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this extension.
* [Example: taking the service into operation without a shell](./examples/commissioning-without-a-shell.md) - A walk-through with fictitious values, and the change of the secret afterwards.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for builders, operators and administering agents: procedure with the choices to decide, rules, acceptance checklist, common mistakes, self-check.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
