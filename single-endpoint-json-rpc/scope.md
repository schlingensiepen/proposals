---
type: Proposal Scope
description: Security considerations, what the concept costs, its preconditions, when this no longer holds, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

| Threat | Typical course | Countermeasure |
|---|---|---|
| **Secret in the logs** | A token is in the query string. It lands in the access log, in the browser history, in the `Referer` sent to third-party servers, and in every error report someone forwards | F7: credentials in headers only. This threat does not arise from an attack but from normal operation — which is why it is the only one that construction can rule out completely |
| **Uploaded script gets executed** | A file with a script extension lands in a directory the web server serves directly and is executed on retrieval | Three barriers (F9), graded by obligation: **required**, the actual content type on intake (application code, always effective); **required**, the default documents against listing; **recommended**, disabled script execution — the most effective one and at the same time the only one that needs a server configuration and can therefore fail silently |
| **An unguessable address is not an authorization** | A file address gets forwarded, is in a browser history on a shared machine, or in a backup. Anyone who has it gets the file | Accept and bound it deliberately (F8): only for content whose loss stays **single**. The difference from a token lies not in the nature — both are bearer credentials — but in the location: a path is logged, a header is not |
| **A directory listing undoes unguessability** | Requesting the directory without a filename shows every identifier at once. "Unguessable" becomes "enumerable", and the randomness from F11 is worthless | `index.html` **and** `index.php` in every served directory (F9, barrier 2). Costs nothing and needs **no server configuration** — the same default-document mechanism as F1. Relying instead on a server setting you cannot verify on someone else's hosting would be the mistake |
| **The provision inherits the attachment's justification** | A backup is stored like an attachment under a permanent address. The reasoning "the damage stays bounded" is silently carried over — for a file containing the entire dataset | F12: commission, poll, fetch, **terminate**. Termination is a method of its own and mandatory for security-relevant data; for critical data additionally an expiry, lazily enforced |
| **Half a file under a valid address** | The address is published while compilation is still running. A caller gets an incomplete backup and cannot tell it from a complete one | F12: the address comes into being **only after completion**. Before that there is a state, not a path |
| **Attacks look like normal traffic** | Everything is `200 POST /api/`. An intermediate server, a rate limiter, or an access-log analysis cannot tell sign-in attempts from search queries | A deliberate price of F4. Anyone who needs detection at that level must keep it in the application — a log of its own with method names and outcomes. That is extra work F4 causes |
| **Talkative error messages** | The plain text of an internal error names paths, table names, library versions | Defensible for a system with one operator, where diagnosability weighs more. With multiple tenants or public access it is **not**: there `error.message` carries only a generic statement for internal errors, and the detail goes to the server log |
| **One endpoint, one checkpoint** | The authorization check is in one place ahead of all methods. A fault there affects **everything** | Which is also the advantage: one place is auditable, twenty are not. The check belongs **before** any resolution of identifiers, or the error message alone reveals whether an object exists |

**What the concept does not protect against:** it says nothing about **who** may
do what. It demands the mechanisms by which a credential comes into being, is
renewed and is individually taken back (F7), and it fixes where it travels and
that it is checked in one place — which policy runs on top of that is for the
application to decide. It does not protect against application faults inside the
methods and is no substitute for input validation.

## What it costs

- **The infrastructure sees nothing any more.** The access log reads `200 POST
  /api/`, always. Status-code-based monitoring, rate limiting by error rate,
  per-endpoint analysis — none of it works.
- **HTTP caching disappears entirely.** Every call is a `POST` to the same
  address. Where responses would be cacheable, you have to build it yourself.
- **Debugging gets less convenient.** No call can be typed into an address bar;
  every diagnosis needs a tool that sends a body.
- **The envelope is less known than REST.** Every new client author reads two
  pages of standard before making the first call.
- **One endpoint has become four entrances.** The thesis says "one endpoint"; in
  fact there are the API, the upload, the binary retrieval, and the provision.
  Four things with four security considerations — F8 to F12 are not a detail but
  **five of the twelve rules and most of the attack surface**.
  If you adopt this concept, you adopt above all these exceptions.
- **Orphaned files.** F9 produces them inevitably; finding them is a directory
  comparison, clearing them a manual step nobody remembers.

## Where it applies

### Preconditions

A web server that executes scripts and resolves a default
document; a `POST` body the script can read; a client that can set headers. No
rewriting, no library, no framework, no package manager. Static files and API
come from the same origin. Few, known clients and one operator.

### When this no longer holds

The answer becomes wrong when:

- **many heterogeneous clients** need flexible queries over deep graphs — then
  GraphQL pays off and the library is worth its price;
- **HTTP caching is needed**, because large, immutable content goes through the
  API — then resource addresses with `GET` are the right answer, and F1 must be
  replaced by an environment with rewriting;
- **infrastructure must see the semantics** — an API gateway, a web application
  firewall, a per-endpoint rate limiter. All of them work on method, path and
  status; behind one endpoint they are blind;
- **responses must be streamed** or run long. The envelope presupposes one
  response in one piece.

### What it does not solve

On access, what this concept **demands** must be told apart from
what it **leaves open**. Demanded are the five mechanisms from F7 — create an
invitation, exchange it for a session credential, renew it, revoke individually,
issue machine keys exactly once. They are a commission to the builder, not an
omission. **What stays open is the policy**: who may invite, how long an
invitation holds, how often it may be redeemed, how many devices a person may
have, whether there are roles, and what happens on loss. That is a decision of
the application and a subject in its own right. It says nothing
about **versioning** the interface (not yet settled, below), nothing about **events in the
reverse direction** (push, streaming, long requests), and nothing about **how
the frontend is delivered** — F10 only fixes that it is the same origin.

### Not yet settled

* **Versioning.** One endpoint has no place for a version, and the envelope
provides for none. Conceivable are a suffix on the method name, a header of its
own, or a second directory next to it. The question is so far unanswered because
there is exactly one client author — which is not an answer but a deferral.
* **What happens when an intermediate server is in front after all?** F4 assumes
the server determines the status itself. A reverse proxy, a load balancer, or an
outage produces its own status codes with its own error pages — then the client
does get a body that is not an envelope. Does F4 need a rule for that case, or
is "whatever is not JSON is a transport error" sufficient?
* **Where exactly is the line on plain text for internal errors?** "One
operator, so the exception may come out verbatim" is a convenient rule. When
does this no longer hold — at the second user, at the first external client, at
the first publicly reachable form?
* **Lazy enforcement — is it enough?** F12 enforces an expiry by having every
call to the provisioning methods clear expired files along the way. That avoids a
scheduled process but has a gap: if nothing is commissioned for a long time,
nothing is cleared either — and that is exactly when a complete copy stays there
longest. Is an additional clearing step needed elsewhere, and where?
* **Does barrier 2 really hold everywhere?** F9 relies on an `index.html`
suppressing the directory listing, because every web server serves a default
document. That holds for the common servers; whether it holds **everywhere** —
including behind object stores, CDNs and delivery layers that treat directories
differently — is untested. The barrier is written as mandatory; its generality is
an assumption.
