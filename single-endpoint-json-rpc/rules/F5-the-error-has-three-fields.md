---
type: Proposal Rule
description: Every error carries a number with HTTP status semantics, a machine-readable type and plain text for humans; all three are mandatory.
number: F5
binding: must
role: independent
applies_to: [builder, consumer]
---

# F5 · The error has three fields with three jobs

**Applies to:** builder, consumer.

**What holds.** `error.code` carries a number with the **semantics of the HTTP
status codes** (400, 401, 404, 409, 410, 415, 500, 503). `error.data.type` names
the concrete case **machine-readably** (`number_taken`, `state_frozen`).
`error.message` explains it in **plain text for humans**. All three are
mandatory.

**Why.** The three fields serve three different consumers, and every attempt to
get by with two harms one of them. The code allows coarse handling without
knowledge of the application — "something about the request was wrong" versus
"not signed in" — and taking over the HTTP semantics saves inventing a numbering
lore: it is established, precise and familiar to everyone. The type is what a
client actually branches on; plain text is unsuitable for that, because it
changes as soon as someone improves the wording. And the plain text is what a
human reads — it may be concrete, with numbers and names in it.

**Obvious and rejected.** *Only code and plain text* — then the client branches
on strings, and the first improved wording breaks it. *Only type and plain text*
— then every client needs the complete list of types before it can even say
"that was an authorization problem". *Own numbers instead of the HTTP semantics*
— you reinvent a lore that already exists, and you have to document it.

**How you notice it is violated.** The client contains a comparison against a message
string.
