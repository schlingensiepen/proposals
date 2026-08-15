---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: single-endpoint-json-rpc
language: en — translated from the German original
sync: v1.0 (2026-08-14)
question: "How do I talk to the server when all I have is cheap PHP hosting — no URL rewriting, no libraries, no framework?"
thesis: "Every call is a POST to a single endpoint; method, parameters and errors live in the body, the credentials in headers — and binary data stays outside."
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

## 1. Problem

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

## 2. The concept

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

## 3. Terms

- **Endpoint** — an address requests go to. Here: exactly one for all
  application logic.
- **Envelope** — the fixed wrapper every request and response sits in; here the
  JSON-RPC 2.0 object with `jsonrpc`, `method`, `params`, `id` and
  `result`/`error` respectively.
- **JSON-RPC 2.0** — a very terse standard for remote calls over any transport,
  finalized in 2010. It defines exactly the envelope and a handful of protocol
  errors; nothing else.
- **Default document** — the file a web server serves when an address points at
  a directory (`index.php`, `default.aspx`). Every server does this in its core,
  without an add-on module.
- **URL rewriting** — a server module rewriting addresses onto other files. Not
  reliably present on cheap hosting.
- **Credential** — a piece of evidence with which a caller shows who or what it
  is: session cookie, API key, administrative key.
- **Handle** — an unpredictable string naming an uploaded file before anyone
  knows what it belongs to.
- **Fragment** — the part of an address after the `#`. The browser **does not
  send it to the server**, so it appears in no access log and no `Referer`; only
  code in the browser sees it.
- **Redemption secret** — a secret presented once and exchanged for a lasting
  credential; typically the token from an invitation link.
- **Provision** — a file compiled on commission that contains much or all of the
  data: a backup, an export. To be distinguished from an **attachment**, which
  is part of the domain data and comes into being with it.
- **Origin** — the combination of scheme, host and port. Two addresses with the
  same origin count as the same source to the browser; that decides about
  cookies and about whether a call needs a preflight.

## 4. Decisions

### F1 · One address, reached through the default document

**What holds.** All application logic hangs on exactly one address that points
at a **directory** (`/api/`) and is resolved to a script by the server's default
document. No path behind the directory, no technology marker, no rewriting.

**Why.** Without rewriting an address is either a file or a directory — there is
no third option, and both major web servers answer `404` to anything else. The
default document is the only resolution rule every server masters in its core;
it is thus the smallest common assumption one can make. That the address
incidentally no longer names a technology is a bonus, not a reason: an
`index.php` in the address turns any later change of language into a breaking
change for every caller.

**Obvious and rejected.** *Paths behind the script file*
(`/api/index.php/items/7`) — works on some configurations and not on others; it
depends on a setting you can neither see nor change on someone else's hosting.
*Application logic in the query string* (`/api/?p=items.search`) — the address
becomes unambiguous, but every parameter lands in the access log, and nobody
keeps the line between "control" and "secret" for years. *Assume rewriting and
document it* — pushes the problem onto the operator and fails on platforms that
do not know the module at all.

**Symptom of violation.** The API runs on one hosting package and answers `404`
on another, with no change to the code.

### F2 · An established envelope, not a homegrown one

**What holds.** The envelope is JSON-RPC 2.0, unchanged: `jsonrpc`, `method`,
`params` as an object, `id`; in the response `result` **or** `error`. No
homegrown format, no extra fields at envelope level.

**Why.** Whoever has one endpoint needs a field saying what is meant — that is
unavoidable once the address says nothing. Inventing that wrapper yourself costs
the same work as adopting a finished one and adds the single disadvantage that
nobody knows it. JSON-RPC has been unchanged since 2010, fits on two pages, and
requires no library: the envelope can be read and written in twenty lines. On
hosting without a package manager, that is the decisive criterion.

**Obvious and rejected.** *A homegrown, "simpler" format* — same cost, without
anyone else knowing it; and the seemingly superfluous fields of the standard get
reinvented one by one at the first error case. *GraphQL* — pays off with many
heterogeneous clients making flexible queries over deep graphs; with a flat
model and few known access patterns you pay for a library and a query language
for nothing, and file uploads are a special case there. *REST with resource
addresses* — the most obvious choice, and it fails on F1: resource addresses
cannot be expressed without rewriting.

**Symptom of violation.** The envelope has gained a field the standard does not
provide for — and a second client ignores it.

### F3 · No batch requests, no notifications

**What holds.** An array in the body is rejected, as is a request without an
`id`. The standard explicitly permits both; neither is used here.

**Why.** A batch request has no overall outcome: it can partly succeed, so every
error handler and every test needs a second branch that does not exist for a
single call. A notification (a request without a response) moves the question
"did it arrive?" into the caller's confidence. Both buy round trips — and round
trips are not the problem a system with one operator and a handful of clients
has.

**Obvious and rejected.** *Leave batch available for the bulk case* — then it
exists, so it gets used, so it must be tested; a capability you do not exclude
is one you maintain. *Notifications for logging* — tempting, but a lost logging
call goes unnoticed, and that is exactly why it is the wrong place to economize.

**Symptom of violation.** A bug report says "half of it went through" — and
nobody can say which half.

### F4 · The HTTP status is `200`; the outcome lives in the envelope

**What holds.** As long as the request was readable as JSON-RPC at all, the
server answers HTTP `200` — including for application-level errors. There are
exactly **two** exceptions: an unreadable body yields `400`, an HTTP method
other than `POST` yields `405`. Both still carry a complete error object in the
body, with `id: null`, because the `id` could not be read.

**Why.** With one endpoint the HTTP status no longer describes what happened,
only whether the envelope arrived. Mixing the two levels — sometimes status,
sometimes envelope — forces every client to handle two error sources and to know
their precedence. The two exceptions are admissible because no envelope existed
yet: whoever sent unreadable JSON also sent no `id`.

That both exceptions nonetheless carry an error object is the actual decision:
**a client never sees an empty body.** Otherwise it needs a special path for
"response without content", and that path is rarely written and even more rarely
tested.

**Obvious and rejected.** *Map application errors onto HTTP status codes* (`404`
for "not found") — looks cleaner and is an illusion: the status then describes
partly the transport and partly the application, and with an intermediate server
that can produce `404` itself the two are no longer distinguishable. *Always
`200`, without exception* — for an unreadable body the server would have to
report success in order to transport a failure; that is a lie at transport
level.

**Symptom of violation.** A client checks the status first and only evaluates
the body "on success" — losing error messages in the process.

### F5 · The error has three fields with three jobs

**What holds.** `error.code` carries a number with the **semantics of the HTTP
status codes** (400, 401, 404, 409, 410, 415, 500, 503). `error.data.type` names
the concrete case **machine-readably** (`number_taken`, `state_frozen`).
`error.message` explains it in **plain text for humans**. All three are
mandatory.

**Why.** The three fields serve three different consumers, and every attempt to
get by with two harms one of them. The code allows coarse handling without
knowledge of the application — "something about the request was wrong" versus
"not signed in" — and taking over the HTTP semantics saves inventing a numbering
lore: it is established, precise and familiar to everyone. The type is what a
client actually branches on; plain text is unsuitable for that, because it
changes as soon as someone improves the wording. And the plain text is what a
human reads — it may be concrete, with numbers and names in it.

**Obvious and rejected.** *Only code and plain text* — then the client branches
on strings, and the first improved wording breaks it. *Only type and plain text*
— then every client needs the complete list of types before it can even say
"that was an authorization problem". *Own numbers instead of the HTTP semantics*
— you reinvent a lore that already exists, and you have to document it.

**Symptom of violation.** The client contains a comparison against a message
string.

### F6 · Protocol errors and application errors are separate number spaces

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

**Obvious and rejected.** *Use the protocol code for "invalid params" for
missing application fields too* — it is called that, after all; that is exactly
where the distinction dies, because then the same code means "your client is
broken" in one case and "the user left a field empty" in the other. *Use the
protocol code for internal errors* — the standard provides it, but in practice
it carries no usable message; an internal error is better served as an
application-level `500` with plain text.

**Symptom of violation.** An empty form field produces the same error code as a
programming fault in the client.

### F7 · Credentials go in headers — never in the body, never in the address

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
this pattern must provide five mechanisms; without them the decision cannot be
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

**Symptom of violation.** The access log contains a line you can sign in with.

### F8 · Binary data leaves the envelope — in both directions

**What holds.** Bytes never go through the JSON envelope. Uploading uses a
**separate multipart endpoint** next to the API; downloading uses an
**unguessable address** the web server serves directly, without any application
code running.

**Why.** JSON carries no bytes. Base64 costs a third in volume — uploading over
a mobile connection, that is exactly the request that hits the time limit.
Memory weighs heavier: a Base64 field must sit in memory in full before anything
happens with it. For a database backup of a few hundred megabytes that pushes
the process past any memory limit a cheap package sets. The web server, by
contrast, serves a file without reading it.

**The price must be named explicitly:** the unguessable address is a
**credential in the shape of a string**. Whoever has it gets the file — there is
no check behind it, no revocation, no expiry a static web server could enforce
by itself.

**That is the same kind of credential as a token — with one difference that
counts.** Every session or machine key is a bearer credential too: whoever holds
it may act. "No revocation, no expiry" applies there just as much, unless
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

**Obvious and rejected.** *Base64 in the envelope, "it is only an image"* — the
volume would be bearable, the memory demand is not; and the line between "only
an image" and "the entire database" drifts upward over time. *Serving through
the API with an authorization check* — then application code runs for every
image, and any overview page with twenty thumbnails starts twenty script
invocations. *Signed addresses with an expiry* — the right solution as soon as a
server can enforce it; without rewriting and without application code in the
serving path it cannot be expressed.

**Symptom of violation.** The server's memory usage depends on payload size
rather than on the number of requests.

### F9 · File intake carries no application logic

**What holds.** The upload endpoint takes **one file** and returns **one
handle**. Nothing else: no reference to an object, no database row. What the
file belongs to is stated afterwards by an ordinary RPC call quoting the handle.

**Why.** The reason is not tidiness but **atomicity**. An operation with three
files happens as: upload three times, then **one** application call with three
handles. If the third upload breaks off, nothing has come into being. The
reverse route — create the object first, then attach files — leaves half an
object behind on every abort; and where history is immutable, that cannot be
cleaned up. The second reason is speed: file intake is the slowest request in
the system, and everything you load onto it lengthens precisely the request most
likely to hit a time limit.

**The area the upload writes into is served directly by the web server** (F8,
F11). That makes it the most dangerous spot in the whole system: an uploaded
script file would be **executed** there on retrieval, and a directory listing
would turn "unguessable" into plain "enumerable". Three barriers, in descending
obligation — and the order matters, because on cheap hosting the lower two can
fail:

1. **Required: check the actual content type**, not the declared one. The
   declared type comes from the caller and is therefore evidence of nothing.
   This check is application code and therefore works **always** — it is the one
   barrier you can rely on.
2. **Required: put an `index.html` and an `index.php` into every served
   directory**, both empty or carrying one sentence. They suppress the directory
   listing **without any server configuration**, because every web server serves
   a default document instead of enumerating a folder — exactly the mechanism
   from F1, used a second time. Two files, because servers differ in which they
   prefer and you do not know which applies. **Without this barrier F8 is
   worthless:** a listing shows every identifier at once, and then nobody has to
   guess.
3. **Recommended: switch off script execution in the target directory.** This is
   the most effective barrier and at the same time the only one that requires a
   **server configuration** — an `.htaccess` the operator may not even have
   evaluated, or a `web.config` on the other platform. On a package whose
   configuration you can neither see nor change, this is a measure you **take
   and do not assume**. Whoever declares it mandatory builds their security on an
   assumption they cannot verify.

**The grading is the point.** Barriers 1 and 2 cost nothing and work everywhere;
barrier 3 works best and can fail silently. That is why none of the three may
stand alone — and why barrier 3 must not be the one you rely on.

**Obvious and rejected.** *Have the upload attach the file right away* — saves
one call and costs atomicity. *Clean up never-attached files on a timer* — such
a rule eventually deletes while someone is still typing; orphaned files are
cheaper than that failure mode, and they are findable by comparing directory
against database.

**Symptom of violation.** After an aborted upload, half an operation sits in the
data.

### F10 · Static delivery and API are one origin to the outside

**What holds.** Frontend files and API live under the same origin, solved via
**directories** below the same base: the application at the root, the API under
`/api/`, binary data under a directory of its own.

**Why.** Two origins immediately mean three extra construction sites: CORS
headers, a preflight before every call with content type `application/json`, and
cookies that count as third-party cookies and vanish depending on browser
settings. None of them is unsolvable; all three are for nothing when they can be
avoided. And avoiding them costs nothing on this platform, because the static
files come from the same server anyway. The cookie credential from F7 works
without special handling only under this condition.

**Obvious and rejected.** *API on its own subdomain* — proper and customary, but
here it costs exactly the three construction sites above for no return; a
separate subdomain pays off when API and frontend scale separately or are run by
different teams. *Static files from a CDN, API from your own server* — sensible
for large, publicly readable content, and then to be solved via a shared origin
with path routing; here everything sits behind a credential, so a CDN would have
nothing it may serve.

**Symptom of violation.** An `OPTIONS` precedes every call in the log — or the
session cookie disappears in a browser with strict defaults.

### F11 · The randomness sits in the directory, not in the filename

> This decision describes the mechanism F8 and F9 use. It sits back here because
> numbers are never reassigned; in reading order it belongs between F8 and F9.

**What holds.** For binary data there is a **writable area the web server serves
as an entirely ordinary file area** — no application code, no special handling.
Within it **every file gets its own subdirectory**, named with a
**cryptographically random** identifier (a UUIDv4 or 128 bits from a secure
random source, in hex). The file inside **keeps its name from the upload**. The
address therefore reads:

```
<base>/<area>/<random-guid>/<original-filename>
```

**Why.** The randomness sits in the directory and not in the filename — for
three reasons that together force the construction:

- **The randomness has to sit somewhere**, or the address is derivable and F8 is
  void. The directory is where there is room for it, because nothing else has to
  go there.
- **The filename stays meaningful.** Whoever saves the file gets
  `invoice-2026.pdf` and not `9f2c4a1e…`. A random filename would need a
  `Content-Disposition` header to heal that — and only someone with application
  code in the serving path can set one. That is exactly what F8 excludes. **The
  construction is therefore not taste but a consequence of F8.**
- **Name collisions are impossible** without anyone renaming anything: two files
  of the same name sit in different directories.

**The identifier must come from a cryptographic random source.** A counter, a
timestamp or a language's usual weak random function makes the address
predictable — and thereby renders F8 worthless without anyone noticing.

**Obvious and rejected.** *Random filename in one shared directory* — saves a
directory, costs the meaningful name; besides, a folder with ten thousand files
is noticeably slow on some file systems. *Structure by date or object*
(`/2026/08/entry-7/`) — makes the address derivable; whoever knows an object
guesses its files. *Content hash as identifier* — tempting, because it merges
duplicates, but the hash is computable: whoever holds the same content knows the
address. *Both versions of a file (original and thumbnail) in one directory* —
tempting, because the folder would then be "worth something even without the
database". It is not: the folders are named after random values; without the
database nobody knows what they belong to, whether one file sits in them or two.

**Symptom of violation.** From one known file address a second can be guessed —
or a request without a filename shows the directory contents.

### F12 · Large compilations: commission, poll, fetch, terminate

**What holds.** When the application has to **compile** larger datasets — a
backup, an export — this does not happen through a fixed path but through
methods on the endpoint:

| Method | Job |
|---|---|
| `…commission` | commissions the compilation. Answers with the download path when produced immediately — otherwise with a **job handle** |
| `…status` | only for concurrent compilation. Reports the state and, **only once finished**, the download path, formed in the usual file pattern (F11) |
| `…terminate` | ends the provision: file and path disappear. **For security-relevant data this method is mandatory**, not optional |

**Additionally, an expiry may be defined for critical data**, after which a
provision lapses even without termination.

**Why.** A provision takes the same route as an attachment (F8, F11) and
therefore looks like one — but the damage is different: an attachment's address
exposes **one attachment**, a backup's address **the entire dataset**. The
justification from F8 — "the damage stays bounded" — therefore does not hold
here, and without a decision of its own the backup would silently inherit a
justification that never applied to it. The envelope cannot carry the bytes, but
it can carry the **commission**; so the control — request, poll, terminate —
stays in the API, and only delivery sits outside. The three paragraphs below
justify the three parts individually.

**Why the address comes into being only after completion.** If it existed
earlier, half a file would sit under a valid address — and a caller cannot tell
an incomplete backup from a complete one. A state is a statement, a path is a
promise; while compilation runs there may only be the statement. This holds
regardless of whether work is concurrent — it is merely unavoidable there.

**Why termination is a method of its own and does not happen automatically.**
The server does not know when a retrieval is finished; an aborted download looks
to it like a successful one. The **retriever** knows. So it reports it, and the
same call clears up. The lifetime thus hangs on an event that is actually
observed, rather than on a clock nobody winds.

**Why the expiry belongs there anyway — and how it is enforceable without a
scheduled process.** If termination does not happen, because a retrieval aborts
or a process dies, a complete copy of the dataset lies around. For critical data
that is unacceptable. But a cheap hosting package has no reliable timer. The
resolution is **lazy enforcement**: every call to one of the three methods
clears expired provisions along the way. It costs nothing and needs nothing — it
has the gap that nothing is cleared while nobody commissions anything (Q4).

**Obvious and rejected.** *Treat the provision like an attachment, i.e. store it
under a permanent path* — convenient, and the actual danger; a backup sitting
permanently under an address is a data leak waiting for that address to end up
somewhere. *Stream the compilation through the envelope* — produces exactly the
memory demand F8 avoids. *Only an expiry, without a terminating method* — then
the file lies around until it lapses although it was fetched long ago; the
expiry is the fallback, not the normal case. *Only termination, without an
expiry, even for critical data* — relies on the confirmation always arriving.
*Return the path at commissioning time and let the caller poll whether the file
is there yet* — moves the distinction "not yet" versus "broken" onto the client,
which cannot make it.

**Symptom of violation.** The provisioning directory contains files from last
week — or a retrieval yields a file that will not open.

**Load-bearing structure.** **F1 carries everything.** If the precondition "no
rewriting" falls, F1 is void, and with it the main reason for F2 — then resource
addresses are possible and REST is a serious alternative. F3 to F6 describe the
envelope and stand or fall with F2; among themselves they are independent, and
F5 and F6 can be adopted individually into any other API. F7 is independent of
everything else and the decision with the widest reach.

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
  bytes do not. If F12 falls away, the backup inherits the attachment's
  justification, which never applied to it — the most dangerous single failure in
  this dossier.

F10 stands on its own but is what makes F7 convenient. **Whoever takes only one
rule takes F7.**

## 5. Security considerations

| Threat | Typical course | Countermeasure |
|---|---|---|
| **Secret in the logs** | A token sits in the query string. It lands in the access log, in the browser history, in the `Referer` sent to foreign servers, and in every error report someone forwards | F7: credentials in headers only. This threat does not arise from an attack but from normal operation — which is why it is the only one that construction can rule out completely |
| **Uploaded script gets executed** | A file with a script extension lands in a directory the web server serves directly and is executed on retrieval | Three barriers (F9), graded by obligation: **required**, the actual content type on intake (application code, always effective); **required**, the default documents against listing; **recommended**, disabled script execution — the most effective one and at the same time the only one that needs a server configuration and can therefore fail silently |
| **An unguessable address is not an authorization** | A file address gets forwarded, sits in a browser history on a shared machine, or in a backup. Whoever has it gets the file | Accept and bound it deliberately (F8): only for content whose loss stays **single**. The difference from a token lies not in the nature — both are bearer credentials — but in the location: a path is logged, a header is not |
| **A directory listing undoes unguessability** | Requesting the directory without a filename shows every identifier at once. "Unguessable" becomes "enumerable", and the randomness from F11 is worthless | `index.html` **and** `index.php` in every served directory (F9, barrier 2). Costs nothing and needs **no server configuration** — the same default-document mechanism as F1. Relying instead on a server setting you cannot verify on someone else's hosting would be the mistake |
| **The provision inherits the attachment's justification** | A backup is stored like an attachment under a permanent address. The reasoning "the damage stays bounded" is silently carried over — for a file containing the entire dataset | F12: commission, poll, fetch, **terminate**. Termination is a method of its own and mandatory for security-relevant data; for critical data additionally an expiry, lazily enforced |
| **Half a file under a valid address** | The address is published while compilation is still running. A caller gets an incomplete backup and cannot tell it from a complete one | F12: the address comes into being **only after completion**. Before that there is a state, not a path |
| **Attacks look like normal traffic** | Everything is `200 POST /api/`. An intermediate server, a rate limiter, or an access-log analysis cannot tell sign-in attempts from search queries | A deliberate price of F4. Whoever needs detection at that level must keep it in the application — a log of its own with method names and outcomes. That is extra work F4 causes |
| **Talkative error messages** | The plain text of an internal error names paths, table names, library versions | Defensible for a system with one operator, where diagnosability weighs more. With multiple tenants or public access it is **not**: there `error.message` carries only a generic statement for internal errors, and the detail goes to the server log |
| **One endpoint, one checkpoint** | The authorization check sits in one place ahead of all methods. A fault there affects **everything** | Which is also the advantage: one place is auditable, twenty are not. The check belongs **before** any resolution of identifiers, or the error message alone reveals whether an object exists |

**What the concept does not protect against:** it says nothing about **who** may
do what. It demands the mechanisms by which a credential comes into being, is
renewed and is individually taken back (F7), and it fixes where it travels and
that it is checked in one place — which policy runs on top of that is for the
application to decide. It does not protect against application faults inside the
methods and is no substitute for input validation.

## 6. What it costs

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
  **five of the twelve decisions and the larger part of the attack surface**.
  Whoever adopts this concept adopts above all these exceptions.
- **Orphaned files.** F9 produces them inevitably; finding them is a directory
  comparison, clearing them a manual step nobody remembers.

## 7. Scope and limits

**Preconditions.** A web server that executes scripts and resolves a default
document; a `POST` body the script can read; a client that can set headers. No
rewriting, no library, no framework, no package manager. Static files and API
come from the same origin. Few, known clients and one operator.

**Tipping conditions.** The answer becomes wrong when:

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

**Not solved.** On access, what this concept **demands** must be told apart from
what it **leaves open**. Demanded are the five mechanisms from F7 — create an
invitation, exchange it for a session credential, renew it, revoke individually,
issue machine keys exactly once. They are a commission to the builder, not an
omission. **What stays open is the policy**: who may invite, how long an
invitation holds, how often it may be redeemed, how many devices a person may
have, whether there are roles, and what happens on loss. That is a decision of
the application and a subject in its own right. It says nothing
about **versioning** the interface (see Q1), nothing about **events in the
reverse direction** (push, streaming, long requests), and nothing about **how
the frontend is delivered** — F10 only fixes that it is the same origin.

## 8. Track record

**A complete operation** — an entry with three images in an inventory system,
from start to finish. *Example; names and values are invented.*

First, file intake three times. It knows neither entry nor purpose:

```
POST /api/upload.php
Content-Type: multipart/form-data; boundary=…
Cookie: session=…

  (field "file": the image file)
```

```json
{"handle": "9f2c4a1e-7b03-4d55-9c18-2e6a0b8f1d34"}
```

Only the application call establishes the connection — **one** call, three
handles, one indivisible outcome:

```
POST /api/
Content-Type: application/json
Cookie: session=…
```

```json
{"jsonrpc": "2.0",
 "method": "states.create",
 "params": {"item": 7,
            "text": "Christmas decorations, upper shelf",
            "photos": ["9f2c4a1e-…", "a71b0c93-…", "c04e58d7-…"]},
 "id": 1}
```

```json
{"jsonrpc": "2.0", "result": {"state": 42}, "id": 1}
```

If the third upload breaks off, **nothing** has come into being — there is no
half entry, only three orphaned files without a database row (F9).

An application error looks like this. The HTTP status is `200`; the three fields
carry three different pieces of information:

```json
{"jsonrpc": "2.0",
 "error": {"code": 409,
           "message": "Number \"12\" is already assigned to entry #7",
           "data": {"type": "number_taken"}},
 "id": 2}
```

A protocol error, by contrast, uses the reserved number space — telling the
client that **its code** is wrong, not its data (F6):

```json
{"jsonrpc": "2.0",
 "error": {"code": -32601, "message": "Unknown method: items.serch"},
 "id": 3}
```

The images are fetched by the browser **not** through the API but directly from
the web server — a randomly named directory in which the file has kept its
original name (F8, F11):

```
GET /images/9f2c4a1e-7b03-4d55-9c18-2e6a0b8f1d34/shelf-top.jpg
```

**And the same route for a backup — with the three additions from F12.**
*Example; values invented.* A backup process commissions the compilation.
Because it takes minutes, no path comes back but a job handle:

```json
{"jsonrpc": "2.0", "method": "backup.commission", "params": {}, "id": 10}
```

```json
{"jsonrpc": "2.0", "result": {"job": "b23f9c01-…", "state": "running"}, "id": 10}
```

While compilation runs there is **no path** — only a state. Completion is what
creates it, in the same pattern as an attachment:

```json
{"jsonrpc": "2.0", "method": "backup.status", "params": {"job": "b23f9c01-…"}, "id": 11}
```

```json
{"jsonrpc": "2.0",
 "result": {"state": "done",
            "path": "/provision/7d4e2a95-…/inventory-2026-08-14.sqlite",
            "size": 41582592},
 "id": 11}
```

The process fetches the file directly from the web server — and **then reports
the successful retrieval**. The same call terminates the provision; file and
path disappear:

```json
{"jsonrpc": "2.0", "method": "backup.terminate", "params": {"job": "b23f9c01-…"}, "id": 12}
```

If the retrieval aborts, the file stays — and is cleared along the way at the
next `backup.commission`, provided its expiry has passed (F12, lazy
enforcement). That is the difference from the image above: its address may
remain, because it exposes an image. This one exposes the entire dataset.

**Reference implementation: omitted, because none exists yet (status: Draft).**
The concept is fully specified and described in contract tests, but the running
application it comes from sits on an older state and does not yet implement it.
The statements in this dossier are therefore **thought through, not proven** —
in particular, the claim that F9 actually establishes atomicity is so far an
argument only.

## 9. Open questions

**Q1 · Versioning.** One endpoint has no place for a version, and the envelope
provides for none. Conceivable are a suffix on the method name, a header of its
own, or a second directory next to it. The question is so far unanswered because
there is exactly one client author — which is not an answer but a deferral.

**Q2 · What happens when an intermediate server does sit in front?** F4 assumes
the server determines the status itself. A reverse proxy, a load balancer, or an
outage produces its own status codes with its own error pages — then the client
does get a body that is not an envelope. Does F4 need a rule for that case, or
is "whatever is not JSON is a transport error" sufficient?

**Q3 · Where exactly is the line on plain text for internal errors?** "One
operator, so the exception may come out verbatim" is a convenient rule. When
does it tip — at the second user, at the first foreign client, at the first
publicly reachable form?

**Q4 · Lazy enforcement — is it enough?** F12 enforces an expiry by having every
call to the provisioning methods clear expired files along the way. That avoids a
scheduled process but has a gap: if nothing is commissioned for a long time,
nothing is cleared either — and that is exactly when a complete copy sits around
longest. Is an additional clearing step needed elsewhere, and where?

**Q5 · Does barrier 2 really hold everywhere?** F9 relies on an `index.html`
suppressing the directory listing, because every web server serves a default
document. That holds for the common servers; whether it holds **everywhere** —
including behind object stores, CDNs and delivery layers that treat directories
differently — is untested. The barrier is written as mandatory; its generality is
an assumption.

## 10. Sources & related work

- **JSON-RPC 2.0 Specification**, the envelope verbatim, including the reserved
  error codes and the optional batch and notification forms this concept
  excludes: <https://www.jsonrpc.org/specification> (retrieved 2026-08-14).
- **RFC 9110 · HTTP Semantics**, section 15, the meaning of the status codes
  whose semantics F5 carries into the error object:
  <https://www.rfc-editor.org/rfc/rfc9110.html> (retrieved 2026-08-14).
- **Same-origin policy and CORS preflight**, the three construction sites F10
  avoids: <https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS>
  (retrieved 2026-08-14).
- **Deepening, not a prerequisite:** the sister dossier
  `single-file-static-frontend`
  (<https://github.com/schlingensiepen/proposals/tree/main/single-file-static-frontend>)
  covers the other side of the same application: how the frontend that talks to
  this API is delivered. **It overlaps with this dossier in two places** — the
  exception for files (F8, F9) and the single origin (F10). There they are
  recommendations, here they are requirements: this dossier describes a concrete
  stack, that one a design pattern. Both are complete on their own; whoever
  reads both reads these two topics twice, at different levels of obligation.
