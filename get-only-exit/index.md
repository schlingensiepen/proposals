---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, what it imports.
* [Concept](./concept.md) - The problem, the concept in prose, how it relates to the rest of a self-describing service, the load-bearing structure, the twelve-step procedure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

Twelve rules in four blocks: the cut (F1 to F3), the operations (F4 to F7), behavior on error and repetition (F8 to F10), price and description (F11, F12). One answer each; give one up and you are no longer using the concept. The address forms in them are patterns.

* [F1 An emergency exit, not a second API](./rules/F1-an-emergency-exit-not-a-second-api.md) - Own prefix, named as a detour, a pass-through with no capability of its own, held by a structural test.
* [F2 Everything in the path, nothing in the query; the secret first](./rules/F2-everything-in-the-path-the-secret-first.md) - The secret is the first path segment on every call; an identifier is an address, not a permission.
* [F3 Two encoding rules](./rules/F3-two-encoding-rules.md) - Identifiers and addresses readable, free text as base64url; a slash reaches the handler and gets the rule.
* [F4 Reading mirrors the main surface; the form follows the caller](./rules/F4-reading-mirrors-the-main-surface.md) - The same facts, text by default, JSON on request, `Vary: Accept`.
* [F5 The upload in parts](./rules/F5-the-upload-in-parts.md) - Open, append numbered parts, finish; the store never sees half a file.
* [F6 The composition](./rules/F6-the-composition.md) - Filled field by field, shown at any time, deposited by exactly one call; no removal.
* [F7 The word: composition, not draft](./rules/F7-the-word-composition-not-draft.md) - And depositing is not finishing.
* [F8 Refusals that work on this surface](./rules/F8-refusals-that-work-on-this-surface.md) - No header, no other verb, no address form from an earlier shape.
* [F9 No guessing](./rules/F9-no-guessing.md) - Contradictory and repeated input is refused or made impossible to express.
* [F10 Repeatability without headers](./rules/F10-repeatability-without-headers.md) - The key in the path, the receipt on the composition.
* [F11 The price, named](./rules/F11-the-price-named.md) - The secret in the address is a breach with five duties; the main surface never offers this way.
* [F12 The description is the only document its caller has](./rules/F12-the-description-is-the-only-document.md) - Prose under the prefix, the chain of identifiers, coupled to discovery, accepted by a reviewer who may only send GET.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, where it applies, where the answer flips, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - The reference implementation, what it proves and what not, incidents stripped down, the honest state.
* [Sources](./sources.md) - What this proposal builds on: one import, four tensions, related proposals, external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent that builds or reviews such a surface, or has to use one as a consumer.
* [Example: a message with attachment, GET only](./examples/walkthrough-message-with-attachment.md) - The way of a reviewing agent through the surface, step by step; values fictional.
* [Extensions](./extensions.md) - Proposals that extend this one. None yet.

# History

* [Update log](./log.md) - Dated changes to this bundle.
