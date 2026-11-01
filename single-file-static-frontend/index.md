---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, the price in short.
* [Concept](./concept.md) - The problem, the concept in prose with its load-bearing structure, the procedures for setting up and for reviewing.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. F7 and F8 are recommendations (should): they concern the interface, to which this cut does not commit. Deployment F1 and F3, security F2, the address F4 and F5, third-party assets F6, the interface F7 and F8. Whoever takes only one rule takes F2.

* [F1 One artifact, not a build result](./rules/F1-one-artifact-not-a-build-result.md) - What gets deployed is exactly what sits versioned in the repository; no bundling, transpiling or minifying.
* [F2 The file is delivered unauthenticated, and therefore carries no data](./rules/F2-delivered-unauthenticated-carries-no-data.md) - The document contains only form and hydration logic; everything of substance arrives through the API; every request yields the same bytes.
* [F3 No application code runs in the delivery path](./rules/F3-no-application-code-in-the-delivery-path.md) - The document comes from a file server; no template is filled, nothing is inserted or assembled.
* [F4 The address names a state of the client, not a resource of the server](./rules/F4-the-address-names-a-client-state.md) - The server knows one entry point and always delivers the same document; which view is shown is the client's business.
* [F5 The state sits in the fragment, so the server need not be bent](./rules/F5-the-state-sits-in-the-fragment.md) - The client state sits after the `#`; no History API with path-like addresses, hence no catch-all rule to deploy.
* [F6 Third-party assets are vendored, not linked](./rules/F6-third-party-assets-are-vendored.md) - Third-party libraries sit as versioned files in the artifact; no script and no stylesheet is loaded from a foreign server.
* [F7 Files and binary data bypass the API envelope (recommendation)](./rules/F7-files-bypass-the-api-envelope.md) - Uploads get a separate intake and downloads come straight from the file server, decided before the first file.
* [F8 Delivery and API under one origin (recommendation)](./rules/F8-delivery-and-api-under-one-origin.md) - Static artifact and API share one origin, unless the separation is bought deliberately.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, when the answer flips, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - An invented walk-through of an inventory application, and why there is no reference implementation yet.
* [Sources](./sources.md) - What this proposal builds on: related proposals and external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent building or reviewing such a frontend: when to apply, rules, recommendations, procedure, acceptance checklist, common mistakes.
* [Extensions](./extensions.md) - Proposals that extend this one. None yet.

# History

* [Update log](./log.md) - Dated changes to this bundle.
