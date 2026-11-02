---
type: Proposal Rule
description: Bytes never go through the envelope; uploads use a separate multipart endpoint, downloads an unguessable address the web server serves directly, accepted only where a loss stays single.
number: F8
binding: must
role: load-bearing
applies_to: [builder]
---

# F8 · Binary data leaves the envelope — in both directions

**Applies to:** builder.

**What holds.** Bytes never go through the JSON envelope. Uploading uses a
**separate multipart endpoint** next to the API; downloading uses an
**unguessable address** the web server serves directly, without any application
code running.

**Why.** JSON carries no bytes. Base64 costs a third in volume — uploading over
a mobile connection, that is exactly the request that hits the time limit.
Memory weighs heavier: a Base64 field must be in memory in full before anything
happens with it. For a database backup of a few hundred megabytes that pushes
the process past any memory limit a cheap package sets. The web server, by
contrast, serves a file without reading it.

**The price must be named explicitly:** the unguessable address is a
**credential in the shape of a string**. Anyone who has it gets the file — there is
no check behind it, no revocation, no expiry a static web server could enforce
by itself.

**That is the same kind of credential as a token — with one difference that
counts.** Every session or machine key is a bearer credential too: anyone who
holds it may act. "No revocation, no expiry" applies there just as much, unless
somebody keeps a block list. The difference lies **not in the nature but in the
location**: a token in a header appears in no access log — a path appears in
every one. This file's address shows up in the server log, in the browser
history, in `Referer` headers and in every intermediate server, that is,
precisely where F7 explicitly does not want a credential.

This is accepted here, and the justification must state both halves: the
**damage stays bounded** — the address exposes one file, not the account and not
the dataset — and the alternative costs one script invocation per thumbnail.
**Where the damage is not bounded, this justification does not hold**; F12
applies there instead. So the procedure is defensible for content whose address
only the authorized ever see and whose loss stays single — and wrong for
everything else.

**How the address is constructed is stated in F11.**

**Considered and rejected.** *Base64 in the envelope, "it is only an image"* — the
volume would be bearable, the memory demand is not; and the line between "only
an image" and "the entire database" drifts upward over time. *Serving through
the API with an authorization check* — then application code runs for every
image, and any overview page with twenty thumbnails starts twenty script
invocations. *Signed addresses with an expiry* — the right solution as soon as a
server can enforce it; without rewriting and without application code in the
serving path it cannot be expressed.

**How you notice it is violated.** The server's memory usage depends on payload size
rather than on the number of requests.
