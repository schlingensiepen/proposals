---
type: Proposal Rule
description: Every credential travels in an HTTP header, never in params, query string or path; the session cookie is exchanged for a redemption secret that arrives through the URL fragment, and five mechanisms are mandatory.
number: F7
binding: must
role: independent
applies_to: [builder, consumer]
---

# F7 · Credentials go in headers — never in the body, never in the address

**Applies to:** builder, consumer.

**What holds.** Every credential travels in an HTTP header. No credential sits
in `params`, none in the query string, none in the path. There are three kinds
with three jobs:

| Kind | shows | who issues it | lifetime |
|---|---|---|---|
| **Session credential** (cookie) | "this person on this device" | the **server**, via `Set-Cookie` | long, individually revocable |
| **Machine key** (own header) | "this process may do this job" | created at setup, shown in clear exactly once | lasting, individually revocable |
| **Administrative key** (own header) | "the operator, during setup" | created when the system is set up | lasting |

The session credential is a cookie with `HttpOnly`, `Secure` and
`SameSite=Lax`. It is renewed on every visit so it cannot expire mid-use.

**Where the session credential comes from.** It is **exchanged**, not handed
out: the caller presents a redemption secret **once** — typically the token from
an invitation link — and the server answers with `Set-Cookie`. From then on the
redemption secret no longer exists for that client.

**How that redemption secret reaches the client without landing in a log:**
through the **fragment** of the address (`…/#invite=<token>`), not the path and
not the query string. **The browser does not send the fragment to the server.**
It therefore appears in no access log, no `Referer` and no intermediate server —
it is the **only place in an address where a secret may sit.** The client reads
it there with JavaScript and sends it **once** in a request body; afterwards it
exists in no address.

**What this demands of the application.** F7 does not merely describe a
transport — it **commissions the builder**. Whoever builds an application on
this pattern must provide five mechanisms; without them the rule cannot be
implemented:

1. **A way to create and hand out a redemption secret** — the invitation. It is
   unpredictable, and the application knows what it entitles its holder to.
2. **A method that exchanges it for a session credential** and sets `Set-Cookie`
   in doing so. It is the only place where a session credential comes into being.
3. **Renewal of the session credential on every visit**, so it cannot expire
   mid-use.
4. **A way to revoke a single credential** — one device, one machine key —
   without affecting the others. That presupposes the credentials are
   **distinguishable**: what you cannot tell apart you cannot block
   individually. Whoever gives all of a person's devices the same value has
   forfeited this mechanism before needing it.
5. **An issuing path for machine keys that shows the clear text exactly once.**
   Afterwards the application holds only a hash; a key that can be read back at
   any time is a permanently readable secret.

**What is not prescribed is the policy**: who may invite, how long an invitation
holds, how often it may be redeemed, how many devices a person may have, whether
there are roles. The application decides that. **The mechanisms above are
mandatory** — they are the precondition for a credential coming into being,
being renewed and being taken back at all.

**Why.** Addresses get logged — in the server's access log, in the browser
history, in intermediate servers, in error reports. A secret there is not
compromised once someone attacks, but **from the outset**, through normal
operation. The body is usually not logged but would be the wrong place for
another reason: credentials belong to the connection, not to the call — sitting
in `params` they would have to be dragged along and checked by every single
method, instead of one place deciding ahead of all methods. One endpoint has a
real advantage here: there is exactly **one** place where the check happens.

**`HttpOnly` is not a detail here:** a cookie JavaScript cannot read also cannot
leak through a flaw in the client and cannot end up in client-side logging by
accident. That is precisely why the session credential is a cookie and not a
value the client manages itself and writes into a header — although the latter
looks tidier at first glance.

**Obvious and rejected.** *Token as a parameter, "it is a POST anyway"* — the
body is not in the log today and may well be tomorrow, as soon as someone turns
on diagnostics; and the check migrates into the methods. *Token in the path,
because it looks so RESTful* — same logging as the query string, plus `Referer`.
*The redemption token in the query string of the invitation link* — the most
obvious route and the worst: it lands in the server's access log, in the
`Referer` and in the browser history, before anyone has done anything wrong. The
fragment costs nothing and avoids all three. *Session credential as a value in
the client's memory, sent in a header* — forfeits `HttpOnly` and with it the
protection against leaking through the client. *Credential on the first call,
state on the server afterwards* — needs session storage a stateless script does
not have.

**How you notice it is violated.** The access log contains a line you can sign in with.
