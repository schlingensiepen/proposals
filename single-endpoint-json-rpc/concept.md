---
type: Proposal Concept
description: The problem of an API on cheap shared hosting without rewriting, libraries or framework, the concept of one POST endpoint with a JSON-RPC 2.0 envelope and an explicitly justified path for everything binary, and its load-bearing structure.
---

# One endpoint, one verb, one envelope

**In one sentence:** The entire API is a single `POST` endpoint speaking
JSON-RPC 2.0; whatever does not fit through a JSON envelope — images, archives,
anything binary — gets its own, explicitly justified path.

**Abstract:** On cheap shared hosting you can rely on neither URL rewriting nor
installable libraries, and every secret in a query string ends up in the access
log. A single endpoint with an established RPC envelope solves all three
problems at once, without a line of third-party code. The price is that HTTP
infrastructure learns nothing about the calls any more.

## Problem

"I have a hosting package for a few euros a month. I can upload PHP files, and
that is it. Whether `mod_rewrite` is enabled I do not know — on the same
provider's Windows package the module does not exist at all. I have no package
manager, hence no framework. And still an API is supposed to run there, spoken
to by a web app and a background process."

Three questions follow from this. They belong together and are usually answered
separately:

- **What do the addresses look like?** Without rewriting, every address is
  either a file or a directory. `/api/v1/items/7` is neither — the server
  answers `404`, on both platforms. That leaves `/api/index.php/…`, which writes
  the technology into the address, or `/api/?p=…`, which writes the application
  logic into the query string.
- **Where do the secrets go?** A token in a query string sits in the access log,
  in the browser history, and in every `Referer` the browser sends along. That
  is not a theoretical risk; that is normal operation.
- **Where do the files go?** A JSON envelope carries no bytes. Base64 costs a
  third in volume and forces the server to hold the whole file in memory — on a
  package with a tight memory limit and a short time limit that is exactly the
  request that fails.

## The concept

There is **one** address for the entire API: `POST <base>/api/`. The web server
resolves it to a script through its default document — every server can do that,
it is not an add-on module. The body carries a **JSON-RPC 2.0 envelope**: the
method name, the parameters as an object, and an `id` that comes back
unchanged. Method names follow the pattern `area.action`, so an alphabetical
list of methods doubles as an outline. The response contains either `result` or
`error`; the HTTP status is **always `200`**, because the application-level
outcome lives in the error object, not in the status line. The error object has
three fields with three jobs: a **number** carrying the semantics of the
familiar HTTP status codes, a **machine-readable type**, and **plain text for
humans**. Credentials — session cookie, machine key, administrative key — travel
exclusively in **headers**, never in the body and never in the address; the
server issues the session credential in exchange for a redemption secret that
arrives through the **fragment** of an address — the one place a browser does
not send to the server and which therefore appears in no log.

Everything binary leaves the envelope, in **three** shapes: **uploading**
through a separate multipart endpoint that accepts only a file and returns a
handle; **downloading** through an unguessable address the web server serves
directly — a randomly named directory in which the file keeps its original name;
and **provisioning** large compilations — backups, exports — which are
*commissioned* through the envelope and yield an address in the same file
pattern only once compilation has finished. The envelope cannot carry the bytes,
but it can carry the **commission**; so the application logic stays in the API
and only delivery sits outside.

**Load-bearing structure.** **F1 carries everything.** If the precondition "no
rewriting" no longer holds, F1 is void, and with it the main reason for F2 — then resource
addresses are possible and REST is a serious alternative. F3 to F6 describe the
envelope and stand or fall with F2; among themselves they are independent, and
F5 and F6 can be adopted individually into any other API. F7 is independent of
everything else and the rule with the widest reach.

**F8 to F12 are the price of F2**, not its supplement: they exist solely because
the envelope carries no bytes. Among themselves they hang together closely, and
the chain runs one way:

- **F11 is the mechanism** F8 merely asserts. Without F11, "unguessable address"
  is a statement of intent.
- **F8 rests on F11**, not the other way round — and F11 rests on F8, which
  forces the randomness into the directory: without application code in the
  serving path there is no `Content-Disposition` header, so the filename must
  stay meaningful.
- **F9 rests on both.** Its second barrier (`index.html`/`index.php`) is not an
  extra but the condition under which F8 holds at all: a directory listing turns
  "unguessable" into "enumerable", and then the randomness from F11 is worthless.
- **F12 is the boundary of F8** and at the same time the only pattern here that
  **joins** envelope and file path: the commission goes through the envelope, the
  bytes do not. If F12 is dropped, the backup inherits the attachment's
  justification, which never applied to it — the most dangerous single failure in
  this concept.

F10 stands on its own but is what makes F7 convenient. **Whoever takes only one
rule takes F7.**

## The procedure

This concept fixes rules, not a build order. The agent guide ([`AGENT.md`](./AGENT.md)) lays out the three tasks it is used for: adding a method, adding a file upload, and reviewing an existing API against the rules.
