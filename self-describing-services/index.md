---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, what changed in 1.1, what it imports, the parts in short.
* [Concept](./concept.md) - The problem, the concept in prose with its load-bearing structure, the procedures for builder and consumer.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. Building the description F1 to F7, the consumer side F8 to F11, the access gear F12 to F14, retrofit F15, arriving at the reader F16 to F20, before setup F21, holding the separation F22.

* [F1 One source: the service describes itself](./rules/F1-one-source-the-service-describes-itself.md) - The authoritative description of a service, entry page, usage description and interface description, is delivered by the service itself at runtime; there is no separately distributed copy.
* [F2 Entry page and cascade: everything linked, decide first, load second](./rules/F2-entry-page-and-cascade.md) - An unauthenticated entry page of about twenty lines suffices for the decision whether the service is needed and lists and links every part of the description.
* [F3 Thematic division with an explained structure](./rules/F3-thematic-division-with-an-explained-structure.md) - The description should be divided into thematic parts along the consumers' concerns, with the structure explained on the entry page; the anchor skill package follows the same division.
* [F4 Discovery without authentication, without secrets](./rules/F4-discovery-without-authentication-without-secrets.md) - Every description step is retrievable with a plain GET, without credentials, in a form the weakest intended reader accepts, and contains neither secrets nor operational internals.
* [F5 Live templating with the real base URL](./rules/F5-live-templating-with-the-real-base-url.md) - All examples are rendered with the host under which the service is currently being addressed.
* [F6 The pumped-up contract](./rules/F6-the-pumped-up-contract.md) - The generated interface description is treated as a product and enriched, every field and operation described with a real example, enforced as a gate in the build.
* [F7 Teaching error messages](./rules/F7-teaching-error-messages.md) - Every rejection names the correct path, concretely, with the next step.
* [F8 The anchor skill: a shell of links, actively offered](./rules/F8-the-anchor-skill.md) - The service provides a storable skill that contains no knowledge, only purpose, target and links, and the entry page actively offers it.
* [F9 One instance, one anchor](./rules/F9-one-instance-one-anchor.md) - Remembering, anchoring and authorizing happen per instance, never per software; when a request is ambiguous, the agent asks.
* [F10 Making available at scopes — with the harness's own means](./rules/F10-making-available-at-scopes.md) - The user makes the service available in natural language at a scope, and the agent uses the storage means of his own harness; the mechanics are not standardized.
* [F11 Delivery duty: the code of conduct travels with the service](./rules/F11-delivery-duty.md) - Every rule that addresses consumers must appear in the service's own usage description, in the service's own words.
* [F12 Narrow tokens as the recommendation for new builds](./rules/F12-narrow-tokens-for-new-builds.md) - A new build should cut access as narrow capabilities, one token, one capability, one place, issued server-side and shown once.
* [F13 Secret handling in a fixed ranking](./rules/F13-secret-handling-in-a-fixed-ranking.md) - The usage description prescribes to the consumer the ranking of `where-a-secret-comes-from`; for the consumer, the once-only channel is the context window.
* [F14 Optional self-registration with a display string](./rules/F14-optional-self-registration-with-a-display-string.md) - A service may offer an open registration method; if it does, it answers with token, display string and flag, and only the display string ever reaches a human.
* [F15 Retrofit: the façade describes, it does not mediate](./rules/F15-retrofit-the-facade-describes-it-does-not-mediate.md) - For an existing API, a description service of its own may deliver the cascade; it renders the real target and never touches the data traffic.
* [F16 The delivery form belongs to the description: weakest reader, announced switch](./rules/F16-the-delivery-form-belongs-to-the-description.md) - Delivery form is part of the description: prose steps default to text/plain, the exact type only on explicit Accept, Vary: Accept; a switch in the form of the answers is announced or avoided.
* [F17 Present tense: the description says what is, not what was](./rules/F17-present-tense.md) - The delivered description names only forms the service knows now; hindsight belongs in the decision records that travel with the source code.
* [F18 Closure: no reference the reader cannot resolve; every identifier is handed out first](./rules/F18-closure.md) - Nothing in the description needs inside knowledge to resolve, and every identifier the caller must substitute comes with the call that yields it, in the same sentence.
* [F19 One word means one thing](./rules/F19-one-word-one-thing.md) - Every word in the vocabulary of the description denotes one thing; unequal consequences carry unequal names.
* [F20 Texts are cut, the entry page links informatively](./rules/F20-texts-are-cut-the-entry-page-links-informatively.md) - Every text is cut so that each step loads only what the next decision needs, and the entry page says at every link what lies behind it.
* [F21 The empty state is decided](./rules/F21-the-empty-state-is-decided.md) - The service names the state between "does not exist" and "here is the description", decides between a setup page with 200 and a 404, and records the decision with its price.
* [F22 Principle and case: every behavior rule stands exactly once, held by machine](./rules/F22-principle-and-case-every-behavior-rule-stands-once.md) - The usage description names the principle once, the operation its case; nothing is spelled out in both places, and a check in the build holds the separation.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs per rule, where it applies, when the answer flips, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - Where it has held: a reference implementation in operation, a precursor in daily use, an antecedent from 2007.
* [Example: the traffic service](./examples/traffic-service-walkthrough.md) - The invented traffic service played through from beginning to end; illustration, not evidence.
* [Sources](./sources.md) - What this proposal builds on: the two concepts it imports, related proposals, external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for builder and consumer: the two procedures, the rules, an acceptance checklist, common mistakes.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
