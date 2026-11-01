---
type: Proposal Agent Guide
name: single-endpoint-json-rpc
description: Self-contained guide for an agent building or reviewing an HTTP API on constrained hosting according to this concept; procedures for adding a method, adding a file upload and reviewing, the rules, an acceptance checklist, common mistakes and a self-check.
---

# Agent guide

## Purpose

Apply when building or reviewing an HTTP API that must run on constrained hosting — no URL rewriting, no package manager, no framework. Also apply when deciding where credentials travel, how error responses are shaped, or how file uploads and binary downloads are handled alongside a JSON API.

This guide gives orders only; the reasoning, costs and limits are in the rules and the scope of this bundle, and nothing here requires reading them. Every rule, step and checklist item is written out in full here; the F numbers in parentheses are provenance markers only.

The whole API is one `POST` endpoint speaking JSON-RPC 2.0. Credentials go in headers. Bytes never go through the envelope.

**When to apply:**

- You are designing or implementing an HTTP API on hosting where URL rewriting, installed
  libraries or a package manager cannot be assumed — typically cheap shared hosting, often
  required to run on both Apache and IIS.
- You are reviewing such an API for consistency.
- You are adding a file upload or a binary download to an existing JSON API.

**Do not apply** when any of these hold: many heterogeneous clients need flexible queries
over deep graphs (use GraphQL); large immutable content must be HTTP-cacheable (use
resource addresses with `GET`); an API gateway, WAF or per-endpoint rate limiter must see
the semantics of each call; responses must be streamed. Each of these makes the rules
below wrong rather than merely inconvenient.

If a rule seems wrong for your case, read the rules and the scope of this bundle before deviating — and record the deviation.

## Procedure

**Adding a method:**

1. Pick a name `area.action`, noun before verb.
2. Define `params` as a flat object. Treat a present-but-empty value (`""`, `[]`) as a
   value, not as a missing field; only an absent key is "field missing".
3. List every error the method can produce, each with its code and its stable type.
   Application errors use the HTTP-style codes only.
4. Ensure the credential check happens before any identifier in `params` is resolved.

**Adding a file upload:**

1. Send the file as multipart to the separate upload endpoint. Return one handle.
2. Sniff the actual content type; reject on mismatch with `415` and type `content_type`.
3. Place both an `index.html` and an `index.php` in the target directory. Where the
   platform allows it, also switch off script execution there — as a measure you take and
   verify, never as a property you assume or rely on.
4. Make the connection in a later RPC call that names all handles of the operation at
   once, so a partial upload leaves nothing behind.

**Reviewing an existing API against this concept:**

1. Confirm there is one address and that it resolves via the default document.
2. Grep the client and server for credentials in query strings, paths or `params`.
3. Confirm every error path produces all three fields, and that no reserved protocol code
   is used for an application error.
4. Confirm no endpoint returns an empty body on failure.
5. Confirm binary data does not pass through the envelope in either direction.
6. Confirm the upload writes no database row.

## Rules

MUST:

1. Expose all application logic at exactly one address that points at a **directory**
   (e.g. `/api/`) and is resolved to a script by the web server's default document
   (`index.php`, `default.aspx`). No path segments behind it, no script name in the
   address. (F1)
2. Use JSON-RPC 2.0 as the envelope, unchanged: request `{"jsonrpc":"2.0","method":…,
   "params":{…},"id":…}`, response with either `result` or `error`, and the `id` returned
   unchanged. Add no fields at envelope level. (F2)
3. Name methods `area.action` — noun before verb — so an alphabetical list of methods
   reads as an outline. (F2)
4. Reject arrays in the body (batch) and requests without an `id` (notification) with the
   protocol error for an invalid request. (F3)
5. Answer HTTP `200` whenever the request was readable as JSON-RPC at all, including for
   application-level errors. Use exactly two other statuses: `400` for an unreadable body
   and `405` for an HTTP method other than `POST`. (F4)
6. Include a complete JSON-RPC error object in the body even in those two cases, with
   `"id": null`. A client must never receive an empty body. (F4)
7. Give every error all three fields: `error.code` as a number carrying HTTP status
   semantics (400, 401, 404, 409, 410, 415, 500, 503), `error.data.type` as a stable
   machine-readable identifier, and `error.message` as plain text for humans. (F5)
8. Keep the protocol's reserved codes (−32768 to −32000) exclusively for protocol faults:
   unreadable JSON, invalid envelope, unknown method, `params` not an object. A missing or
   implausible application field is `400`, never the protocol code for invalid params. (F6)
9. Carry every credential — session cookie, machine key, administrative key — in an HTTP
   header. Issue the session credential yourself via `Set-Cookie`, with `HttpOnly`,
   `Secure` and `SameSite=Lax`, in exchange for a one-time redemption secret the caller
   presents once. (F7)
9a. Deliver that one-time redemption secret to the client in the **fragment** of a URL
    (`…/#invite=<token>`), never in the path or query string. The browser does not send
    the fragment to the server, so it appears in no access log and no `Referer` — it is
    the only place in an address where a secret may sit. The client reads it with
    JavaScript and sends it once in a request body; afterwards it exists in no address.
    (F7)
9b. Provide all five credential mechanisms; without them this instruction cannot be
    followed. (1) A way to create and hand out a redemption secret — the invitation,
    unpredictable, with the application knowing what it entitles its holder to. (2) A
    method that exchanges it for a session credential and sets `Set-Cookie` — the only
    place a session credential comes into being. (3) Renewal of the session credential on
    every visit, so it cannot expire mid-use. (4) A way to revoke one single credential —
    one device, one machine key — without affecting the others; this requires the
    credentials to be distinguishable, so never give all of a person's devices the same
    value. (5) An issuing path for machine keys that shows the clear text exactly once,
    after which only a hash remains. What these mechanisms are *used for* — who may
    invite, how long an invitation holds, how many devices a person gets, whether there
    are roles — is the application's policy and is not prescribed here. (F7)
10. Check credentials in one place ahead of all methods, and check them **before**
    resolving any identifier, so an error message cannot reveal whether an object exists.
    (F7)
11. Accept uploads through a separate multipart endpoint that takes one file and returns
    one unpredictable handle — no object reference, no database row, no schema check. (F9)
12. Protect every directory the web server serves with three barriers, in this order of
    obligation. **Required:** verify the **actual** content type of an uploaded file
    (sniffed, never the declared header) — this is application code and therefore always
    works. **Required:** place both an `index.html` and an `index.php` in every served
    directory; they suppress directory listing with **no server configuration at all**,
    because every web server serves a default document instead of enumerating a folder.
    Without this, a listing turns "unguessable" into "enumerable" and rule 13 is void.
    **Recommended where the platform allows it:** switch off script execution in that
    directory via server configuration — the strongest barrier, and the only one that can
    silently fail on hosting whose configuration you can neither see nor change. Never
    make it the one you rely on. (F9)
13. Serve binary downloads from an unguessable address the web server delivers directly,
    with no application code in the serving path. Build that address as
    `<base>/<area>/<random-guid>/<original-filename>`: one directory per file, named with
    a **cryptographically random** identifier (UUIDv4 or 128 bits from a secure random
    source), with the file keeping its upload name inside. The randomness must sit in the
    directory, not in the filename — otherwise the download loses its meaningful name and
    would need a `Content-Disposition` header, which requires the application code this
    rule forbids. Never derive the identifier from a counter, a timestamp, a content hash,
    or a non-cryptographic random function. (F8, F11)
13a. Treat an unguessable address as a bearer credential: whoever holds it may fetch the
     file. It differs from a token not in kind but in **location** — a path is logged, a
     header is not. Accept it only where the loss stays **single**: one attachment, not the
     whole dataset. (F8)
13b. For large compiled datasets — a backup, an export — do not use a fixed path. Provide
     three methods on the endpoint: one that **commissions** the compilation and returns
     either the download path (if produced immediately) or a job handle; one that reports
     **status** and yields the download path **only once compilation has finished**; and
     one that **terminates** the provision, deleting file and path. The terminating method
     is mandatory for security-relevant data. For critical data additionally define an
     expiry, enforced lazily — every call to these three methods clears expired provisions,
     so no scheduled process is needed. Never publish the path while compilation is still
     running: a caller cannot tell an unfinished file from a corrupt one. (F12)
14. Put static frontend files and the API under the same origin, separated by directories
    below one base. (F10)

NEVER:

- Never put a credential in a query string, in a path segment, or in `params`. Addresses
  are logged by the server, the browser history, intermediate servers and error reports —
  a secret there is exposed by normal operation, not by an attack. (F7)
- Never map application-level outcomes onto HTTP status codes. (F4)
- Never let a client branch on `error.message`; that is what `error.data.type` is for. (F5)
- Never send bytes through the JSON envelope, Base64-encoded or otherwise. It costs a
  third in volume and forces the whole payload into memory. (F8)
- Never have the upload endpoint attach the file to anything. The connection is made
  afterwards, by an ordinary RPC call quoting the handle — that is what makes a multi-file
  operation atomic. (F9)
- Never delete never-attached uploads on a timer; such a rule eventually deletes while
  someone is still working. Find them by comparing directory against database and remove
  them deliberately. (F9)
- Never assume URL rewriting is available, and never require paths behind a script file.
  (F1)
- Never put a redemption secret in the query string of an invitation link. It reaches the
  access log, the `Referer` and the browser history before anyone has done anything wrong.
  Use the fragment. (F7)
- Never let the client hold the session credential as a value it manages itself and sends
  in a header; that forfeits `HttpOnly` and with it the protection against leaking through
  the client. (F7)
- Never store a backup or export under a permanent address. Its address exposes the whole
  dataset, so the justification that applies to attachments — "the loss stays single" —
  does not apply to it. (F12)

## Acceptance checklist

- [ ] Is there exactly one address for all application logic, resolved via the default
      document, with no path behind it and no script name in it? (F1)
- [ ] Does every request and response use the unmodified JSON-RPC 2.0 envelope, with the
      `id` returned unchanged? (F2)
- [ ] Are batch arrays and requests without `id` rejected? (F3)
- [ ] Is the HTTP status `200` for all application-level errors, with only `400`
      (unreadable body) and `405` (wrong HTTP method) as exceptions? (F4)
- [ ] Does every failure — including those two — return a body containing a JSON-RPC error
      object? (F4)
- [ ] Does every error carry code, machine-readable type, and human-readable message? (F5)
- [ ] Are the reserved protocol codes used exclusively for protocol faults, and does a
      missing application field yield `400`? (F6)
- [ ] Does no credential appear in any address or in `params`, anywhere in client or
      server? (F7)
- [ ] Does the credential check run in one place, before any identifier is resolved? (F7)
- [ ] Do all five credential mechanisms exist — create an invitation, exchange it for a
      session credential, renew, revoke individually, issue a machine key exactly once?
      (F7)
- [ ] Are credentials distinguishable enough that one device can be blocked without
      affecting another? (F7)
- [ ] Does the upload endpoint return only a handle, writing no database row? (F9)
- [ ] Are the two required upload barriers in place — content-type sniffing and both
      default documents in every served directory — with disabled script execution added
      where the platform allows it? (F9)
- [ ] Do binary downloads bypass application code entirely? (F8)
- [ ] Is the randomness in the directory name, cryptographically sourced, with the file
      keeping its original name inside? (F11)
- [ ] Does every served directory contain both an `index.html` and an `index.php`? (F9)
- [ ] Is disabling script execution treated as a measure taken, not a property assumed?
      (F9)
- [ ] Do backups and exports go through commission / status / terminate, with the path
      appearing only after completion and a terminating method that deletes it? (F12)
- [ ] Is there an expiry for critical provisions, and is it enforced on every call to
      those methods rather than by a scheduled job? (F12)
- [ ] Has the permanence of unguessable download addresses been accepted deliberately for
      this content, given there is no expiry and no revocation? (F8)
- [ ] Do static files and API share one origin? (F10)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| `404` returned for "object not found" | Return HTTP `200` with `error.code: 404` in the envelope |
| Client checks HTTP status and ignores the body on non-200 | Parse the body first; the status only tells whether the envelope arrived |
| Client switches on `error.message` | Switch on `error.data.type`; the message is for humans and will be reworded |
| Missing form field answered with the protocol code for invalid params | Use `400` with a type; reserved codes mean "your code is broken", not "your data is" |
| Token appended as `?token=…` for a quick test | Move it to a header before it reaches any log; test tokens leak like real ones |
| Image uploaded as a Base64 string inside `params` | Move to the multipart endpoint; memory usage must not scale with payload size |
| Upload endpoint creates the database row | Have it return only a handle; the row is created by the later RPC call |
| Empty body returned on a parse error | Return `400` with a full error object and `"id": null` |
| Declared content type trusted on upload | Sniff the actual type; a declared header is caller-controlled |
| All of a person's devices share one credential value | Issue a distinct credential per device; otherwise revoking one revokes all |
| Machine key readable from the application after creation | Store only a hash; show the clear text exactly once at creation |
| Redemption token placed in `?invite=…` | Move it into the fragment; the browser never sends the fragment to the server |
| Random filename in one shared directory | Move the randomness into a per-file directory; the filename must stay meaningful without a `Content-Disposition` header |
| Directory protected only by disabled script execution | Add `index.html` and `index.php`; the configuration-based barrier can fail silently on hosting you do not control |
| Backup written to a fixed path like `/backup/latest.sqlite` | Commission it, publish the path only when complete, and delete it on termination |
| Download path returned while the export is still being written | Return a status instead; publish the path only after completion |
| API placed on a separate subdomain "for tidiness" | Put it under the same origin unless it must scale separately — otherwise you buy CORS, preflight and third-party-cookie problems for nothing |

## Self-check

Before you report completion: can you name the one address and how it resolves, show that no credential sits in any address or in `params`, show that every failure returns a complete error object, name every directly served directory with its barriers, and say for every backup or export how it is commissioned, fetched and terminated? If any of these is missing, you are not done.
