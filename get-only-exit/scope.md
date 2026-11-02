---
type: Proposal Scope
description: Security considerations of a surface that takes the secret in the address and takes effect on GET, what it costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

This surface is, in the author's words, "the ultimate hack", and its
security considerations begin with the admission that it does two things
an interface should not do: it accepts a secret in the address, and it
takes effect on `GET`. Both are intended, both are bounded, and the
bounds are what make this table checkable. Where a row goes beyond the
built implementation, it says so.

| Attack / risk | Typical course | Countermeasures |
|---|---|---|
| Secret in access logs | Proxy, server, load balancer record the full address; the log is backed up, passed on, searched | price named at the door ([F11](./rules/F11-the-price-named.md)); narrow secret with one target (F11); separate secret per surface, short-lived, easily rotated (F11, beyond the implementation); shortening the secret segment in own logs and a note to the operator for the upstream stations (F11, beyond the implementation) |
| Secret in the caller's transcript and history | the address is in the chat history, in the tool's history, in summaries; it is shared or exported | the description says so before the first call; the service's consumer rules (never repeat, never against another target) apply unchanged; the narrow secret bounds what a shared transcript opens |
| Secret in caches | a shared cache keeps answers under their address and serves them to another caller | `Vary: Accept` on every answer ([F4](./rules/F4-reading-mirrors-the-main-surface.md)); marking the surface's answers as not to be stored (F11, **not built**, see the evidence) |
| Secret in redirects | an answer points to an external address; the tool follows it and sends the calling address along as origin | the surface answers without pointers to external targets and does not redirect; marking answers so that the origin is not passed on is an addition this concept does not require (not yet settled, below) |
| Accidental fetch that takes effect | link preview, prefetching, a tool repeating a fetch | exactly one call takes effect, all others fill a composition ([F6](./rules/F6-the-composition.md)); the effecting call requires a key no prefetcher has and answers a repetition with the same receipt ([F10](./rules/F10-repeatability-without-headers.md)); anyone who has the depositing address has the secret anyway |
| Writing `GET`s and intermediaries | a station repeats a `GET` because it takes it to have no consequence | repeated parts have no consequence, repeated field calls are idempotent (set) or additive only for recipients (append; a repetition yields a duplicate recipient, visible when shown, F6); the depositing call is protected by the key (F10) |
| Identifier as permission | someone knows a composition or upload identifier and tries to fill or read it | the secret is mandatory on every call ([F2](./rules/F2-everything-in-the-path-the-secret-first.md)); someone else's identifiers answer like non-existent ones |
| Guessing secrets through the address | systematically trying the first segment | the same check as on the main surface: high-entropy tokens, *unauthorized* without distinguishing "wrong" from "unknown"; rate limiting is the operator's business |
| Silent guessing by the service | two inputs for one field, repeated parameters | the separation into routes makes ambiguity inexpressible; contradictions are refused ([F9](./rules/F9-no-guessing.md)) |
| Wrong remedy in refusals | a taken-over error text recommends a header; the agent tries it, fails, loses trust | own refusals per surface, test in the build ([F8](./rules/F8-refusals-that-work-on-this-surface.md)) |
| Half files in the store | an upload breaks off; the store takes what exists to be complete | parts lie in the staging area and are handed over only on finishing; gaps are named and refused ([F5](./rules/F5-the-upload-in-parts.md)) |
| Staging area as a dump | abandoned uploads and compositions pile up | the sweeper of the staging area clears after a fixed period; nothing abandoned ever takes effect (F5, F6) |
| Reviewer with a secret that takes effect | the external reviewer of [F12](./rules/F12-the-description-is-the-only-document.md) really deposits | a narrow secret on a target where a slip costs nothing; on this surface in addition: a secret revoked after the review, because it is in the logs of the test environment |

Limit of the model: against an attacker who reads the logs of an upstream
station that the operator does not control, nothing on this surface
protects except the scope of the secret and its lifetime. The concept
makes the damage small and short; it does not make it impossible.
If you cannot bear that, you do not build this surface; "When this no longer
holds" below names the situation.

## What it costs

This section lists what the concept costs. Every item is tied to the
rule that causes it and stands here without offsetting.

**The secret is in the address (F2, F11).** It is therefore in
every access log on the way, in every history, in every transcript, in
every cache that keeps addresses, in the `Referer` header of any request
a tool sends after following a link out of an answer, and in backups of
all of these that nobody looks through any more. Everything the series
otherwise says about handling secrets does not hold on this surface;
what it offers instead
is a named price, a narrow scope and a shorter lifetime. If you build
the surface, you pay this item permanently and for every call, not once.

**Fetches that take effect violate the meaning of the verb (F5, F6).**
`GET` promises the network that it has no effect, and this surface
breaks the promise at known places. Intermediaries, prefetchers and
tools may repeat, anticipate and keep `GET`s, and they do. The design
limits the consequences to filling a composition; it does not remove
that a recipient is appended twice when a tool repeats the call, and
that the caller sees this only if it shows the composition before
depositing.

**Two encoding rules (F3).** The caller must know which value travels
readable and which encoded, and it must be able to encode. A surface for
weak tools thereby demands an ability not every weak tool has; the
description mitigates this with a copyable line; it does not remove it.

**The upload is tedious (F5).** Roughly three kilobytes per fetch: a
document of three hundred kilobytes is a hundred fetches, a large
attachment does not go. Every fetch is a log line with the secret in it.
The main surface's upper limit cannot be reached this way, and the
description must say so instead of letting the caller notice after the
ninetieth fetch.

**A third place to keep in step (F1).** Every operation that changes
changes on the main surface, on the command line, and now on the second
surface. The structural test prevents a second implementation from
arising there; it does not prevent a call from being forgotten there or
a text from going stale. The surface is a pass-through, but a
pass-through has one opening per operation, and each wants maintenance.

**The refusals are texts, and texts age (F8).** Every refusal of this
surface must name a way a caller can go without headers and without a
request body, and because these ways are in the address, every change
to the address form changes every text that names one. A test that asks
for the name of the recommended call does not notice; it sees a right
name in a wrong shape and is satisfied. The author paid this item: after
the rebuild from the query part to the path, three refusals remained
that write the way without the secret segment, and no test reported it.
If you build this surface, you check the refusals for their address form
and not only for their content, and you pay that again with every change to
the form.

**A prose description by hand beside the routes (F12).** The
description of the second surface does not fall out of the code like a
generated contract; it is a text someone writes and that ages beside the
routes. With it the service has, at this place, again what
self-description otherwise does away with: a separately maintained
description, only in the same artifact, delivered at runtime, and with
tests that demand individual sentences. Whether that suffices is not yet
settled (below).

**The staging area becomes state (F5, F6, F10).** Parts, compositions
and receipts lie on disk, need a sweeper, a fixed period and the rule
that none of it ever slips into the store. A service that had no staging
area gets one; one that had one gets two new kinds of residents in it.

**Every round checks twice (F12).** The external check that applies to
every self-description is repeated for this surface with a reviewer that
may only send `GET`. That is a second run per change, a second secret
for it and, because it is in the logs of the test environment, a
revocation afterwards.

**Wording is normative (F7).** One word for two things does not strike
the builder; it strikes the first reader, and then the renaming is due,
across specification, code, tests and description. The author paid for
it once; if you build this surface, you pay it for every word not first
shown to a reader without inside knowledge.

## Where it applies

**Conditions.**

* *A main surface that would exist without this one.* The second surface
  is an emergency exit and presupposes a door: an interface with the
  secret in a header whose operations exist as callable functions, so
  that a pass-through can invoke them (F1). Where only this surface
  existed, it would not be a breach but the rule, and this concept would
  not apply. You recognize the situation by nobody being able to say
  which surface is the first.
* *A secured connection.* The service must be reachable over TLS, more
  urgently on this surface than elsewhere, because without encryption
  the secret is not only in logs but on the wire. You recognize the
  situation by a base URL without `https`.
* *A staging area with a sweeper.* Parts and compositions need a place
  where incomplete things may lie and disappear after a fixed period,
  without anyone writing a second cleanup (F5, F6). You recognize the
  situation by the service having no place where it may delete.
* *A caller that can send addresses of a few kilobytes and can encode.*
  The surface demands long addresses and base64url (F3, F5). A tool that
  cuts addresses at two thousand characters or cannot encode text does
  not get through here either; "What it does not solve" names that.
* *A reviewer that may only send `GET`.* Acceptance under F12 needs an
  agent without inside knowledge that is forbidden the stronger verb.
  You recognize the situation by every available reviewer reaching for
  `POST` as soon as something sticks.

**When this no longer holds.**

* *The secret has high damage potential.* Meant is an access whose leak
  opens more than a target where a slip is bearable: administrative
  power, broad rights, a target system with consequences for third
  parties. The concept no longer holds here because F11 declares the price bearable only
  on condition that it is small and short; such a secret never belongs
  in an address, and for such a service this surface does not exist. You
  recognize the situation by hesitating, when issuing the secret, to
  write it into a log.
* *The callers can send headers after all.* Meant is an environment in
  which it turns out that the supposedly weak tools can set an
  `Authorization` header, if only through a setting. The concept no longer holds here
  because F1 provides the surface only for the case that nothing else
  works; anyone who uses it anyway pays the price of F11 for nothing. You
  recognize the situation by a caller traveling on the second surface
  although the first would have worked.
* *Upstream stations that the operator does not know.* Meant is a way
  through proxies, load balancers or third-party platforms whose logs
  the operator cannot see, shorten or delete and whose retention they do
  not know. The concept no longer holds here because the security considerations name
  exactly this attacker as the limit, and F11 duty (5) can clean only
  one's own stores. You recognize the situation by nobody being able to
  say who keeps the access logs of the public name, and for how long.
* *A policy that forbids secrets in addresses.* Meant is a set of rules,
  contractual, regulatory or internal, that prohibits credentials in
  URLs. The concept no longer holds here because this surface does exactly that; then
  the surface does not exist, and the tool type stays excluded. You
  recognize the situation by the sentence in the policy.
* *Payloads that do not fit into parts.* Meant is a service whose
  effecting operation regularly needs megabytes: images, data sets,
  archives. The concept no longer holds here because F5, at three kilobytes per fetch,
  allows no serious transfer of such data, and the surface would then
  only pretend to offer a function. You recognize the situation by the
  typical call on the second surface falling apart into hundreds of
  fetches.

**What it does not solve.**

* *Issuing and handling the secret.* How a caller gets its secret, how
  it is rotated and revoked, how the consumer keeps it whenever it does
  not put it into an address: other concepts of the series cover that
  (see the sources), and this one changes only one of their rules at one
  place. F11 recommends a separate secret per surface; how the operator
  issues and marks it, this concept does not say.
* *Large files.* The surface transports kilobytes, not megabytes. The
  limit is the address: three kilobytes is what one fetch can carry *in*
  (F5); what a reading call brings *back* is what the main surface
  returns for the same identifier (F4), so reading a large set of data
  is a sequence of small reading calls over what the main surface
  already lists, never an archive file. Anyone who needs both can upload
  a file over the main surface and reference it over the second surface
  (same secret, same identifier),
  but that presupposes that some tool of the caller can send `POST`; if
  so, the question arises why it uses the second surface at all.
* *Callers that cannot do long addresses or base64url either.* For them
  this emergency exit is too narrow as well. Whether there is a surface
  below it (shorter addresses, a dictionary of predefined values), this
  concept does not answer.
* *Undo.* There is no removing from a composition and no recalling a
  deposited request; both are intended (F6) and not a gap, but anyone who
  needs them does not find them here.
* *Reading out results.* The reading calls mirror what the main surface
  reads: the state of an operation, facts about a file. What the main
  surface does not give, the second surface does not give either.
* *The behavior of intermediaries.* Whether a proxy repeats `GET`s,
  whether a cache respects `Vary`, whether a tool shortens addresses:
  that is measured per environment; this concept names the measures the
  service can take, and no more.

**Not yet settled.**

* Whether the service should enforce the marking of secrets for this
  surface: a token carries a mark "for the emergency exit" when issued,
  and only tokens so marked are accepted on the second surface; or
  whether the recommendation in F11 suffices. Enforcing makes the breach
  visible on the server side as well and revocable on its own; it costs
  an attribute in the store, a command and one more refusal that must
  explain why a valid token does not hold here.
* Whether the depositing call needs a second safeguard. The design of F6
  limits the consequences of an accidental fetch to filling a
  composition; the depositing call is protected by the key against
  repetition, not against prefetching by someone who knows the address,
  who, however, has the secret. A one-time mark assigned by the service,
  which the caller receives only when showing the composition, would
  keep a prefetcher from forming the depositing address before the user
  has seen the state. Or that is security against an attacker who has
  everything anyway.
* Whether there is a surface below this one for callers that cannot
  form long addresses either (shorter parts, a dictionary of predefined
  values), or whether this is the point at which a tool does not reach
  the service. Three kilobytes per fetch is the limit addresses set on
  payload.
* Whether the description of the second surface should be generated
  from the same source as the routes (a register of calls from which
  description and handlers arise), or whether handwritten prose with
  tests for individual sentences is the right form, because its reader
  needs sentences and not a table.
* How the review order for the GET-only reviewer must be worded so that
  it also represents the sandboxes that strip the query part: does it
  run from inside such a sandbox, or is that simulated? And whether every
  round should include a run that does the same task over both surfaces
  and compares the results, as a behavioral check alongside the
  structural test of F1.
* How long the service remembers a key after the sweep. The link from key
  to composition is kept with the receipt, and the receipt ends with the
  sweep, so what happens to a key used again after that is not settled,
  although [F10](./rules/F10-repeatability-without-headers.md) counts on
  protection beyond the sweep when it rejects the composition's identifier
  as the key.
* Whether it is enough that the surface's answers point to no external
  targets, so that this concept requires no marking against passing the
  origin on; or whether a tool follows pointers that are in payload
  (a link in a text body that the surface returns when showing the
  composition) and then carries the address with the secret there as
  origin.
