---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, state of version 1.0.
* [Concept](./concept.md) - The problem of two kinds of secrets, the concept of one script per component and one directory per instance with its load-bearing structure, and the procedure in six steps.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. Separation and secrets F1 to F2, protection of the store F3 to F5, window and order F6 to F7, tooling F8 to F10.

* [F1 One script per component, one directory per instance](./rules/F1-one-script-per-component-one-directory-per-instance.md) - The script is versioned and knows no target; the instance directory lies outside version control, knows exactly one and is the only parameter.
* [F2 The script owns the secrets of the system](./rules/F2-the-script-owns-the-system-secrets.md) - The script creates, places and backs up the system secrets beside the instance configuration; no human makes one up.
* [F3 What is delivered is a list from version control](./rules/F3-a-list-from-version-control-is-delivered.md) - Not the contents of the working directory; protective files are versioned explicitly.
* [F4 Runtime data is locked firmly, not exempted by configuration](./rules/F4-runtime-data-is-locked-not-exempted.md) - The lock is in the core of the script, not in a configuration file.
* [F5 The production configuration is created, never overwritten](./rules/F5-the-production-configuration-is-created-never-overwritten.md) - Except with a `--force` that is never the default and names what it destroys.
* [F6 Accesses for other components arise in a time-limited window](./rules/F6-accesses-for-other-components-arise-in-a-time-limited-window.md) - Plain text exactly once; revocation at any time without a window.
* [F7 The order follows the flow of secrets](./rules/F7-the-order-follows-the-flow-of-secrets.md) - The tool enforces it; it matters only on the first delivery per instance.
* [F8 One entry point that runs, creates and lists](./rules/F8-one-entry-point-that-runs-creates-and-lists.md) - The sample configuration belongs to the script, not to the entry point.
* [F9 Every script checks from outside what it has done](./rules/F9-every-script-checks-from-outside-what-it-has-done.md) - What only shows from outside; a finding has an exit code of its own.
* [F10 Every kind of delivery comes with a guide for manual operation](./rules/F10-every-kind-of-delivery-comes-with-a-manual-guide.md) - Use the script, and if that does not work, follow the guide.

# Choices

This concept has no choices; every entry is a rule.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when the answer flips, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - An invented three-component example from the first run to a compromised machine, a second example, and the state without a reference implementation.
* [Sources](./sources.md) - The alternative and the related proposals; no imports, no external sources yet.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent adopting this concept: the steps with the duties of adoption, rules, acceptance checklist, common mistakes.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
