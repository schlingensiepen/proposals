---
type: Proposal Agent Guide
name: get-only-exit
description: Self-contained guide for an agent that builds or reviews a GET-only emergency exit for a service, or that has to use one as a consumer because its only tool is a URL fetcher.
---

# Agent guide

## Purpose

Apply this guide when a web service must be callable by tools that can only send
`GET` (no headers of their own, no request body, often no query string) and you
build or review a second, deliberately restricted surface for them; or when, as an
agent, your only tool is a URL fetcher and the service you were given offers such
a surface. This guide is complete on its own. The rule files are the reference
behind it, named in parentheses; you never need to open them to act correctly.

Two roles use this guide; apply the rule set for yours.

- **Builder**: you run or build a web service whose main interface takes its secret in
  an `Authorization` header and its writes in a request body, and you have callers that
  can send a `GET` and nothing else: link fetchers in chat products, web-fetch tools in
  sandboxes, "fetch URL" blocks of automation platforms, preview and import functions.
  You are deciding whether to build a second surface for them, building it, or reviewing
  one.
- **Consumer**: you are an agent, your only way to the network is fetching a URL, and
  the service you were given offers a surface where everything goes through `GET`.

The address forms below (`/get/v1/<token>/…`, `compose`, `deposit`, `finish`) are
patterns, not a house convention: the words are interchangeable, the shape is not:
secret first, then the thing, then the action, then the value.

## Procedure

For the builder:

1. **Confirm the need.** Name the concrete tools your callers use that can send only
   `GET` without headers. If they can set an `Authorization` header after all, stop:
   the second surface would cost its price for nothing. Stop as well, and say why to
   the user, if any of these holds: the secret that would travel in the address opens
   more than a target where a slip is bearable (administrative power, broad rights,
   consequences for third parties); a policy that applies to the service forbids
   credentials in addresses; nobody can say who keeps the access logs of the public
   name in front of the service, and for how long; the typical effecting call would
   need megabytes and fall apart into hundreds of fetches. Make sure the base URL is
   `https`. (F1, F11)
2. **Choose the prefix** and write the one sentence for the entry page. (F1)
3. **Make sure every operation of the main surface exists as a callable function**
   apart from its HTTP handler; the second surface will call those. Write the
   structural test before the first handler. (F1)
4. **Lay out the path shape:** prefix, secret, thing, action, value. Decide per segment
   whether it travels readable or as base64url; list the segments that could contain a
   slash and route them greedily. (F2, F3)
5. **Build the reading calls** as mirrors with `Accept`-negotiated form, text by
   default, `Vary: Accept`, refusals in the one error format. (F4)
6. **Build the staging area** (or reuse the service's existing one): parts and
   compositions live there, are swept after a fixed period, never reach the store.
   (F5, F6)
7. **Build the chunked upload:** open, part by number, finish, then the same upload
   function. (F5)
8. **Build the composition:** create, one route per field, show, deposit with a key,
   then the same depositing function; keep the receipt on the composition. Check the
   vocabulary with a reader without inside knowledge before the names are in the
   code. (F6, F7, F10)
9. **Write the refusals for this surface**, then the test that scans them for the
   other surface's words and checks their address form. (F8, F9)
10. **Write the prose description:** price first, then encoding, then the form of
    answers, then the calls, each answer-giving call introducing the identifier it
    hands out; add the tests for the identifier chain and for the sentences a caller
    would otherwise have to guess. Couple the surface to the discovery switch and say
    so at both switches. (F11, F12)
11. **Decide the token policy with the operator:** a separate secret per surface if
    possible, shorter life, known at issue. Add `Cache-Control: no-store` to the
    answers and log shortening for the secret segment. Do not decide this for the
    operator. (F11)
12. **Run the GET-only external check;** treat every finding as a defect; record how
    the run represented the sandboxes that strip the query part; revoke the test
    secret afterwards. Repeat after every change. (F12)

For the consumer, the consumer rules below are the procedure.

## Builder rules

MUST:

1. Build the second surface only for callers that cannot send anything but `GET`
   without headers of their own, put it under its own prefix, call it a detour in its
   own description, keep the main surface the first one, and have the service's entry
   page point to it with one sentence that says what it is for ("if you can only send
   `GET`, read on there"). (F1)
2. Make the second surface a pass-through: every call on it invokes the very function
   the main surface uses for the same purpose; it implements nothing itself and offers
   no capability the main surface lacks. The only exception is collecting the parts of
   a chunked upload, and that collecting belongs to the storage layer, not to the
   surface. Hold this with a structural test in the build that fails when a handler on
   this surface performs an operation instead of calling it, and that requires by name
   that the depositing call goes through the same function as on the main surface. (F1)
3. Require no query parameter, for anything. The secret is the first path segment after
   the prefix and is present on every call — including calls that name an identifier,
   because an identifier is an address, not a permission. A missing or wrong secret is
   refused as unauthorized, and the refusal names the path form, not a header. (F2)
4. Use two encoding rules and say so in the description: identifiers, e-mail addresses,
   numbers and names travel readable (ordinary percent-encoding, and a name may not
   contain a slash); free text — anything that may contain a slash: a subject, a body,
   the content of a file part — travels as base64url (alphabet with `-` and `_`, no
   padding). Route every segment that could contain a slash greedily, so the handler is
   reached and answers with the rule instead of a bare 404. Show the encoding with one
   copyable line. (F3)
5. Mirror the main surface for reading: the same objects under the same identifiers
   with the same facts. Choose the form by `Accept`: `application/json` gets the main
   surface's structure unchanged; anything else — `*/*`, no header — gets the same facts
   as `text/plain`, one `name: value` per line, built from the same data. Send
   `Vary: Accept` on every such answer. Refusals stay in the service's one error format
   regardless of `Accept`. (F4)
6. Take files in parts: one call opens an upload and answers its identifier; one call
   per part appends it with the part's **number** before the data; one call finishes,
   assembles the parts in numeric order and hands the bytes to the same upload function
   the main surface uses — only then does a file exist in the store, and the answer is
   the main surface's file record. A repeated part with the same number and the same
   content is harmless; the same number with different content is refused. A gap in the
   numbers is refused, and the message names the missing part. Parts stay in a staging
   area that never becomes the store and is swept after a fixed period. State the
   scale in the description: roughly three kilobytes of payload per call, about a
   hundred calls for a 300 KB document, and the main surface's upload limit is not
   reachable this way. (F5)
7. Build every write that needs a structured body as a composition: one call creates
   it and answers its identifier; one route per field sets or adds a value — fields
   that hold several values (recipients) append one per call, single-valued fields
   replace; one call shows the composition as it stands; exactly one call deposits it,
   handing the finished composition to the same function the main surface uses.
   Nothing leaves the server before that call, and that call is where the user's
   confirmation belongs. Provide no way to remove anything from a composition; a
   caller who errs starts a new one, nothing abandoned is ever deposited, and an
   abandoned composition expires in staging. Refuse to deposit a composition that is
   not yet a valid request and list what is missing. Say in the description that there
   is no removal. (F6)
8. Name the thing built on the server a composition (or another word of your own that
   means only that), never "draft": "draft" names at most a state the result may have at
   the target system. Name the depositing call the way the effecting operation is named
   everywhere else in the service, and never after the call that finishes an upload —
   finishing an upload closes a local collection and has no consequence, depositing
   acts on a foreign system and cannot be undone. Unequal consequences carry unequal
   names. Before publishing the surface, let a reader without inside knowledge read the
   vocabulary for double meanings and treat every question as a finding. (F7)
9. Word every refusal in terms this surface has. No refusal on this surface may
   recommend a header, another verb, a call of the main surface, or an idempotency key
   in a header — even where that text is correct on the main surface. Unauthorized
   names the path form of the secret; not-found names the call on this surface that
   hands out the identifier; an unknown action on a composition lists the actions there
   are instead of pointing at a contract document. Where one text must serve both
   surfaces, name the action rather than the path ("the call that deposits a message").
   Hold this with a test that checks the refusals of this surface for the other
   surface's words. (F8)
10. Never resolve contradictory or repeated input silently. Where two inputs for one
    field are conceivable (a text as a value or as a file), make them two routes so the
    question cannot be asked; where a field holds several values, take one per call and
    append; refuse a repeated upload part with different content; refuse anything whose
    meaning the service would have to guess, and say what was ambiguous. (F9)
11. Carry the idempotency key as the last path segment of the depositing call and pass
    it to the same function that takes it from a header on the main surface: the same
    key with the same secret produces the same identifier and no second effect. Require
    the key to be path-safe (letters, digits, `-`, `_`, `.`) and tell callers to choose a
    fresh one per intended effect, not per attempt. Keep the receipt of the depositing
    call on the composition so that any further depositing call on it answers the same
    receipt — whatever key that call names, because the receipt sits on the object and
    not on the key — until staging sweeps the composition. Say both halves in the
    description: the receipt guards one composition against a second deposit, the key
    guards against depositing two different compositions twice. (F10)
12. Name the price at the door, before the first call, in full sentences: the token is
    the first part of every address and therefore ends up in access logs, in histories
    and in the transcript of whatever is calling; the main surface takes it in a header
    and does not have that cost; use this surface because your tooling leaves you no
    choice, not because it is easier. (F11)
13. Never let the main surface accept a secret from an address — not in the path, not
    in the query string, under no name. (F11)
14. Accept on this surface only narrowly cut secrets: one capability at one place, with
    a target where a leak does bounded harm. A secret with broad rights or
    administrative power never belongs in an address. (F11)
15. Know, as the operator, that a secret will be used on this surface when you issue it,
    and treat it accordingly: shorter-lived, rotated more readily, revoked without
    hesitation. Where you can, issue a separate secret for the second surface rather
    than the one that travels in a header on the main surface. (F11)
16. Keep addresses out of every store this surface controls: mark its answers as not to
    be cached, shorten the secret segment in the service's own logs, and give the
    operator the sentence that the logs of stations in front should do the same. (F11)
17. Serve the surface's own description under its prefix as prose, without a secret,
    rendered with the base URL of the request, as `text/plain` unless a more specific
    type is explicitly asked for, with `Vary: Accept`. Do not rely on a machine-readable
    contract instead: a caller who cannot send `POST` usually cannot work one. In that
    prose, hand out every identifier the text tells the caller to substitute, in the
    same breath as the call that answers with it ("answers `upload_id`"); state the
    upload scale, the staging period, the irreversibility of the depositing call, the
    absence of removal and the two encoding rules; and point at nothing the caller
    cannot process. Switch the surface off together with the service's open
    self-description: where discovery is off, this surface answers not-found. Say
    so at both switches: the one for the self-description and the description of
    this surface. (F12)
18. Treat the surface as accepted only after an agent that has nothing but its
    address, a secret and a task in the user's language — and that is expressly
    forbidden to send anything but `GET` — has completed the task through it and
    reported its way. Repeat that run whenever the surface or its description changes.
    Use a narrowly cut secret on a target where a slip costs nothing, and revoke it
    afterwards, because it now stands in the logs of the test environment. Record
    how the reviewer represented the sandboxes that strip the query part: whether it
    ran from inside one or whether that was simulated. (F12)

NEVER:

- Never add the GET-with-secret-in-the-address form to the main surface "for
  convenience". (F1, F11)
- Never give the second surface a capability the main surface does not have, and never
  implement an operation on it instead of calling the existing one. (F1)
- Never require a query parameter on the second surface. (F2)
- Never let an identifier alone open anything without the secret. (F2)
- Never percent-encode free text that may contain a slash into a path segment; encode
  it as base64url. (F3)
- Never let a part of an upload become a store entry before the upload is finished.
  (F5)
- Never let more than one call on a composition take effect at the target system. (F6)
- Never reuse the main surface's refusal texts on the second surface. (F8)
- Never apply a precedence rule ("file wins over text") or "last value wins" — refuse
  or make the ambiguity inexpressible. (F9)
- Never delete a composition on deposit so that a repeated call answers "not found".
  (F10)
- Never accept a secret with administrative power or broad rights on this surface.
  (F11)
- Never publish the surface without its price in the first paragraphs of its
  description. (F11, F12)
- Never accept the surface on the builder's own walk-through; the reader who counts
  cannot send `POST`. (F12)

## Consumer rules

MUST:

1. Read the surface's description first and pass its price on to your user in one
   sentence before the first call: the token will stand in every address you fetch,
   therefore in logs, histories and this transcript. (F11)
2. Use this surface only because your tooling leaves you no choice. If you can set an
   `Authorization` header, use the main surface instead. (F1, F11)
3. Keep the token out of everything except the addresses you fetch: never quote it in
   your answers, never write it into files that are not your secret store, never use
   it against another host than the one it was issued for. (F11)
4. Encode free text (subject, body, file content) as base64url without padding; send
   identifiers, addresses and numbers readable; never put a slash into a name. (F3)
5. Upload files in numbered parts and finish the upload before referencing the file;
   expect about three kilobytes per call and do not attempt large attachments. (F5)
6. Build the composition field by field, one recipient per call, show it to your user,
   and call the depositing address only after their confirmation — that is the one call
   that cannot be undone. (F6)
7. Choose a fresh idempotency key per message you intend to send, not per attempt; if a
   call's answer was lost, repeat the same address and expect the same receipt. Never
   reuse a key you have used before: a new composition deposited under an old key
   answers the old identifier and quietly sends nothing. (F10)
8. If you set a wrong field, start a new composition; nothing you abandon is sent.
   (F6)

NEVER:

- Never treat the depositing call as "save as draft"; it acts on the target system.
  (F7)
- Never send two values for one field in one call, and never assume a repeated value
  was kept. (F9)
- Never follow a refusal that tells you to set a header or use another verb — report it
  as a defect of the service instead. (F8)

## Checklists

**Acceptance**

- [ ] Does the second surface live under its own prefix, does the entry page point to
      it in one sentence, and is the main surface unchanged? (F1)
- [ ] Does a structural test fail when a handler on this surface performs an operation
      instead of calling it, and does it require by name that deposit goes through the
      one depositing function? (F1)
- [ ] Is there no query parameter anywhere, is the secret the first path segment on
      every call, and does a foreign identifier with a valid secret answer like a
      non-existent one? (F2)
- [ ] Do a subject with a slash, a file name with a slash and a key with a slash each
      answer with the rule rather than a bare 404? (F3)
- [ ] Do the reading calls answer text by default, JSON on request, both with
      `Vary: Accept`, refusals in the one error format either way? (F4)
- [ ] Does the store contain nothing until finish, is a gap named, is a repeated part
      harmless and a differing one refused, and is staging swept? (F5)
- [ ] Does exactly one call on a composition take effect, is there no removal, and does
      an unfinished composition list what is missing? (F6)
- [ ] Do the built thing and the depositing call carry names that cannot be confused
      with a state at the target or with finishing an upload? (F7)
- [ ] Does no refusal on this surface mention `POST`, `Authorization`, a header name
      or a call that only the main surface has — and does every refusal's address form
      match the surface's current shape, secret segment included? (F8)
- [ ] Can two values for one field not be expressed, and does one recipient per call
      accumulate? (F9)
- [ ] Does repeating the depositing address answer the same receipt and leave one
      message at the target? (F10)
- [ ] Do the first paragraphs of the description name the price, and does the main
      surface refuse a secret from an address? (F11)
- [ ] Are only narrowly cut secrets accepted, is a separate secret per surface issued
      where possible, and do answers carry `Cache-Control: no-store`? (F11)
- [ ] Does the description hand out every identifier it asks the caller to substitute,
      state scale, expiry, irreversibility, no-removal and both encodings — and does it
      disappear together with discovery, and is that coupling stated at both switches? (F12)
- [ ] Has an agent restricted to `GET` completed a real task through the current shape
      of the surface and reported its way, and is it recorded how that run represented
      the sandboxes that strip the query part? (F12)

**Conditions (from the scope)**

- [ ] Is the base URL `https`?
- [ ] Is every secret accepted on this surface one whose leak opens no more than a
      target where a slip is bearable?
- [ ] Does no policy that applies to the service forbid credentials in addresses?
- [ ] Can the operator say who keeps the access logs of the public name, and for
      how long?
- [ ] Does the typical effecting call fit into tens of fetches rather than hundreds?

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| `?token=` on the main surface "so simple clients work" | Own prefix, own surface; the main surface never takes a secret from an address (F1, F11) |
| Secret and values as query parameters on the second surface | Everything in the path; many sandboxes strip the query string on GET (F2) |
| Percent-encoding a subject that contains `/` | base64url for free text; an encoded slash is decoded before routing and the route no longer matches (F3) |
| Answers in JSON while the description is prose | Text by default, JSON on `Accept: application/json`, `Vary: Accept` (F4) |
| Writing upload parts straight into the file store | Stage them; hand the assembled bytes to the ordinary upload on finish (F5) |
| Part order taken from arrival | The number in the path is the part's number; assemble numerically (F5) |
| One field with `v=` and `file=` | Two routes; with both given, the service guessed (F9) |
| `to?v=a&v=b` keeps only `b` | One recipient per call, appending (F9) |
| Calling the thing being built a "draft" | Composition; "draft" is at most a state at the target (F7) |
| Naming deposit `finish` "like the upload" | Different consequence, different name (F7) |
| Reusing the main surface's problem texts | Refusals in this surface's terms; a test scans them for `POST`, `Authorization`, header names (F8) |
| Refusal texts left over from an earlier shape of the surface (address without the secret segment) | Test the address form in refusals, not only the call name (F8) |
| Deleting the composition on deposit | Keep it with its receipt; a repeat must answer the same receipt (F10) |
| Same secret on both surfaces, no mark on issue | Separate, short-lived secret for the exit; operator knows at issue (F11) |
| No `Cache-Control` on answers whose address holds the secret | `no-store` on every answer of this surface (F11) |
| Description in the OpenAPI document only | Prose under the prefix; the caller cannot work a contract document (F12) |
| Placeholder `MESSAGE_ID` used twice, never handed out | "Answers `message_id`" at the depositing call; test the introduction, not the mere mention (F12) |
| Builder walks the surface through himself and calls it accepted | An agent restricted to `GET`, with nothing but URL, secret and task (F12) |

## Self-check

Before you report completion as a builder: could an agent that has nothing but the
surface's address, a narrowly cut secret and a task, and that may send nothing but
`GET`, finish the task without guessing anything, and did one actually do so on the
current shape of the surface? Does the description name the price before the first
call? Does the main surface refuse every secret that comes in an address? If any of
these is no, you are not done.

As a consumer: did your user hear the price before the first call, and did they
confirm the composition before you called the one address that deposits it?
