---
type: Proposal Rule
description: Every description step is retrievable with a plain GET, without credentials, in a form the weakest intended reader accepts, and contains neither secrets nor operational internals.
number: F4
binding: must
role: independent
applies_to: [builder]
---

# F4 · Discovery without authentication, without secrets

**Applies to:** builder.

**What holds.** Entry page, usage description, and interface description are retrievable without credentials and contain neither secrets nor operational internals — and this via a simple **GET**, in a delivery form that even the weakest intended reader accepts (F16), detached from whichever dialect the API itself speaks: even a service that offers GraphQL or JSON-RPC delivers the description as plain, linkable GET resources. Where the open disclosure is a nuisance, it can be switched off per tenant — as a deliberate trade-off, not as a default. A builder who nevertheless wants a lock on the door MAY put minimal protection in front of it: an obscurity token in the entry page's URL, which on rendering (F5) is encoded into all onward links. This is deliberately weak protection — URL components end up in logs and histories, and such a token is not a secret — and the recommendation remains: in the simplest case, completely open.

**Why.** Discovery has to work *before* the consumer has credentials — otherwise nobody can find out how to obtain any. And what is retrievable by everyone may contain nothing that not everyone is allowed to know; this content rule is easier to check than any access rule.

**Considered and rejected.** *The description behind a login* — chicken and egg: the way to access presupposes access. *"Internals do no harm, only the customer sees them anyway"* — open endpoints have no customer list.

**How you notice it is violated.** Onboarding instructions circulate outside the service again — in wikis and chats — because the service itself does not hand them out; or the open description contains something an operator would rather delete on reading it.
