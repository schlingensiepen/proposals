---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, what changed in 1.3, the parts with links, what it imports.
* [Concept](./concept.md) - The problem, the concept in prose with its load-bearing structure, the eleven-step procedure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. Design F1 to F6, separation F7 to F10, outer edge F11, state F12 to F13.

* [F1 One service, one function](./rules/F1-one-service-one-function.md) - A service carries exactly one function, describable in one sentence; main and helper methods serve it and share one access; a second independent function is a second service.
* [F2 One artifact, several modes](./rules/F2-one-artifact-several-modes.md) - Service and commands are the same executable artifact, binary, package or directory, with the mode chosen at invocation; a minimum set of modes; no separate administration tool.
* [F3 Commands are scriptable](./rules/F3-commands-are-scriptable.md) - Every command ends with a fixed, documented exit code, offers machine-readable output where it lists, and never demands interactive input it cannot take as an argument.
* [F4 One delivery artifact, one delivery](./rules/F4-one-deliverable-one-delivery.md) - Exactly one delivery artifact is built and published; commands run against it in the same environment; the deployment file names the published artifact instead of building locally; the deliverable says which revision it was built from.
* [F5 One configuration, one judgment](./rules/F5-one-configuration-one-judgment.md) - All modes read the same configuration with the same code and the check command is a mode that passes the start's judgment; precedence and secrets are imported from two sister concepts.
* [F6 The state lies outside the artifact](./rules/F6-state-lies-outside-the-artifact.md) - Everything that must survive a restart, configuration and state, is kept at named, separate places outside the artifact, so a version change carries nothing along and leaves nothing behind.
* [F7 The running service has no management surface](./rules/F7-no-management-surface-in-the-running-service.md) - The running service offers no administration endpoints, debug routes or management UI; diagnosis, inspection and change are commands, authenticated by server access.
* [F8 The permission cut runs between the modes](./rules/F8-the-permission-cut-runs-between-the-modes.md) - Configuration is written only by commands and read by the service mode; operating data including the store is written by both; enforced through users and file permissions in the process form.
* [F9 Configuration and access come into being in one act](./rules/F9-configuration-and-access-in-one-act.md) - The commands configure the function for the specific case and create the access for exactly that configuration in one act; secrets of a variant are persisted in the store, never referenced.
* [F10 The service knows no administrative rights](./rules/F10-no-administrative-rights.md) - There is no administrative user, access or permission model; administration is configuration, and no issued access carries administrative power.
* [F11 No TLS in the service, local endpoint](./rules/F11-no-tls-in-the-service-local-endpoint.md) - The service listens on a local endpoint; public names, certificates and TLS are the business of what the deployment places in front of it, usually a ready-made reverse proxy.
* [F12 Configuration versus state: what arises in operation is state](./rules/F12-configuration-versus-state.md) - The boundary between configuration and state runs at the point in time; the variants and their accesses are state, the store, created by the artifact itself with the concurrency mechanics of maildir-style-file-queues.
* [F13 Data migration runs at start](./rules/F13-data-migration-runs-at-start.md) - A new version migrates data of an older structure in the service mode at start, before it listens, and records the structure version; a rollback finds converted data and says so.

# Choices

More than one legitimate answer; decided by anyone who adopts the concept, and recorded.

* [W1 Channel of the commands](./choices/W1-channel-of-the-commands.md) - Commands started locally by somebody with server access, or brought in over another channel that re-establishes authentication and the permission cut.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when this no longer holds, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - Where it has held: the draft-filing service from beginning to end, and the origin.
* [Sources](./sources.md) - What this proposal builds on: the concepts it imports, related proposals, external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.
* [Example: the imap-sync request](./examples/request-imap-sync.md) - The user description that the context-free building tests of this concept ran against.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent adopting this concept: the eleven steps with the duties of adoption, rules, acceptance checklist, common mistakes.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
