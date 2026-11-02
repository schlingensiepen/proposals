---
type: Proposal Rule
description: Whenever the request was readable as JSON-RPC the HTTP status is 200; only an unreadable body (400) and a method other than POST (405) differ, and both still carry a complete error object.
number: F4
binding: must
role: independent
applies_to: [builder, consumer]
---

# F4 · The HTTP status is `200`; the outcome lives in the envelope

**Applies to:** builder, consumer.

**What holds.** As long as the request was readable as JSON-RPC at all, the
server answers HTTP `200` — including for application-level errors. There are
exactly **two** exceptions: an unreadable body yields `400`, an HTTP method
other than `POST` yields `405`. Both still carry a complete error object in the
body, with `id: null`, because the `id` could not be read.

**Why.** With one endpoint the HTTP status no longer describes what happened,
only whether the envelope arrived. Mixing the two levels — sometimes status,
sometimes envelope — forces every client to handle two error sources and to know
their precedence. The two exceptions are admissible because no envelope existed
yet: anyone who sent unreadable JSON also sent no `id`.

That both exceptions nonetheless carry an error object is the actual rule:
**a client never sees an empty body.** Otherwise it needs a special path for
"response without content", and that path is rarely written and even more rarely
tested.

**Considered and rejected.** *Map application errors onto HTTP status codes* (`404`
for "not found") — looks cleaner and is an illusion: the status then describes
partly the transport and partly the application, and with an intermediate server
that can produce `404` itself the two are no longer distinguishable. *Always
`200`, without exception* — for an unreadable body the server would have to
report success in order to transport a failure; that is a lie at transport
level.

**How you notice it is violated.** A client checks the status first and only evaluates
the body "on success" — losing error messages in the process.
