---
type: Proposal Rule
description: A secret that must reach the client through an address sits after the "#", never in path or query string, and is sent once in a request body; the credential then travels as a server-set cookie.
number: F9
binding: must
role: independent
applies_to: [builder]
---

# F9 · Secrets in addresses sit in the fragment, or nowhere

**Applies to:** builder.

**What holds.** If a secret must reach the client via an **address**, an invitation token or an entry key, it sits after the `#`. Not in the path, not in the query string. From there the application reads it and sends it **once** in the body of a call; afterwards it appears in no address.

**Why.** Everything before the `#` the browser sends to the server, and the server writes it into the access log. It also sits in the browser history and in every `Referer` the browser sends to foreign sites. A secret there is not compromised when somebody attacks, but **from the outset**, through normal operation. The browser does **not** send the fragment; it is the only place in an address where a secret may sit.

The credential itself then travels not in the address but as a cookie the **server** sets. That the server sets it and not the JavaScript is no detail: a cookie the client cannot read does not leak through a flaw in the client and does not end up in logging by accident (F4).

**Obvious and rejected.** *Token in the query string, "it is only a link"*: the most obvious route and the worst; it distributes the secret across three logs before anyone has done anything wrong. *Token in the path*: the same, plus `Referer`. *Manage the credential in the client and send it as a header*: looks tidier and forfeits the protection against the client giving it away.

**How you notice it is violated.** The access log contains a line you can get in with.
