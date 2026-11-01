---
type: Proposal Rule
description: The document comes from a file server; no template is filled, no script runs server-side, nothing is assembled.
number: F3
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F3 · No application code runs in the delivery path

**Applies to:** builder, operator.

**What holds.** The document comes from a file server. No template is filled, no script runs server-side, nothing is assembled.

**Why.** What is not executed cannot be executed wrongly. With the server-side template a whole class of faults disappears — embedded foreign content, wrongly escaped values, template injection — and not because someone avoids it carefully, but because the place does not exist. That is the difference between a measure that must be maintained and a property of the construction.

It is also the precondition for delivery being able to move elsewhere: a file server, an object store, a CDN — for an artifact without application code these are interchangeable options.

**Obvious and rejected.** *A small template, "just for the title"* — it creates exactly the place at issue, and the next requirement puts something substantive into it. *Server-side pre-rendering for search engines* — right if the page is meant to be found publicly; for an application behind a credential it is delivery to nobody.

**How you notice it is violated.** The document contains a place where a value is inserted.
