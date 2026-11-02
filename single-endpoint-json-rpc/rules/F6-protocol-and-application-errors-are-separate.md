---
type: Proposal Rule
description: The codes reserved by the standard stay exclusively with the protocol, application errors use only the HTTP-style codes, and no code appears in both roles.
number: F6
binding: must
role: independent
applies_to: [builder, consumer]
---

# F6 · Protocol errors and application errors are separate number spaces

**Applies to:** builder, consumer.

**What holds.** The codes reserved by the standard (−32768 to −32000) stay
exclusively with the protocol: unreadable JSON, invalid envelope, unknown
method, `params` not an object. Application errors use exclusively the
HTTP-style codes. No code appears in both roles.

**Why.** The line runs between **form** and **content**: "that is not a
parameter structure" is something other than "the field `number` is missing".
Mixing them leaves a client unable to decide whether it has a fault in its
*code* or in its *data* — and that is the most important distinction while
searching. In practice: a missing application field yields `400`, not the
protocol error for invalid parameters.

**Considered and rejected.** *Use the protocol code for "invalid params" for
missing application fields too* — it is called that, after all; that is exactly
where the distinction dies, because then the same code means "your client is
broken" in one case and "the user left a field empty" in the other. *Use the
protocol code for internal errors* — the standard provides it, but in practice
it carries no usable message; an internal error is better served as an
application-level `500` with plain text.

**How you notice it is violated.** An empty form field produces the same error code as a
programming fault in the client.
