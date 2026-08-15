---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: single-file-static-frontend
language: en — translated from the German original
sync: v1.0 (2026-08-15)
question: "How do I cut an application so that deploying it stays a copy operation — and it still gives nothing away?"
thesis: "The frontend is a single static file anyone may have; the address names no resource on the server but a state of the client."
---

# One file anyone may have

**In one sentence:** The frontend is **one** artefact — an HTML file with its
vendored means — delivered without authentication and therefore allowed to
contain no data; everything of substance it fetches at runtime through an API.

**Abstract:** Whoever assembles a frontend from a build result, a server
template and a routing configuration deploys three things that must match. A
single static artefact turns deployment into a copy operation — and because
anyone may have it, nothing may be in it that anyone should not have. The price
is uglier addresses, no server-side preparation, and an empty first screen until
the first response arrives.

## 1. Problem

"I want an application in the browser, and I want to deploy it without thinking
every time. Today my frontend consists of a build result, a few templates the
server fills, and a rewrite rule so the addresses look nice. Three things that
must match — and when I add a view, I have to remember to update the server
configuration. On a second target system I start over."

Two questions hide in that, and they belong together:

- **What exactly gets deployed?** As long as the answer is "files plus a
  configuration plus a build step", deployment is a procedure with ordering,
  states and failure modes — not copying.
- **What is in it that not everyone may see?** As soon as the server fills
  templates with user data, the delivered page differs per request, and who gets
  it is decided by a check somebody must have built correctly.

## 2. The concept

The frontend is **one** HTML file. Everything it needs — styles, scripts,
third-party libraries — sits next to it, versioned and unmodified; there is no
build step turning sources into a result. This file is delivered **without
authentication**, since it is the very thing that obtains a credential in the
first place. So nothing may be in it that not everyone may see. **The markup
carries only form and hydration logic** — the empty structure and the code that
fills it; everything of substance arrives at runtime through an API. **No
application code runs in the delivery path** — no server rendering, no template
that inserts anything; a file server suffices.

This shifts what an address means: it no longer names a **resource on the
server** but a **state of the client**. The server has exactly one entry point
and always delivers the same document. The desired state therefore sits in the
**fragment** — after the `#` — which the browser never asks the server about.
Third-party libraries are vendored rather than loaded from foreign servers. And
whatever does not fit through a JSON interface — images, archives — gets a route
of its own.

## 3. Terms

- **Artefact** — what gets deployed. Here: the contents of a directory of
  versioned files, with no intermediate step.
- **Build step** — a procedure turning source files into a different, deployable
  result (bundling, transpiling, minifying).
- **Data-free markup** — delivered HTML containing no substantive content, only
  structure and labels; every request yields the same bytes.
- **Hydration logic** — the code in the artefact that fills the empty structure
  with data from the API at runtime. It is part of the form, not of the content:
  it describes **where** something goes, not **what**.
- **Fragment** — the part of an address after the `#`. The browser **does not ask
  the server about it**; it is the client's business alone.
- **History API** — the browser interface with which an application changes the
  displayed address without reloading. The addresses so produced look like paths
  and are requested from the server on the **next** call.
- **Catch-all rule** — a server configuration that serves the same document for
  arbitrary paths.
- **Entry point** — the one address the document sits at.

## 4. Decisions

### F1 · One artefact, not a build result

**What holds.** What gets deployed is exactly what sits versioned in the
repository: an HTML file and the files beside it. There is no step that turns
sources into something else — no bundling, no transpiling, no minifying.

**Why.** A build step is a state between source and deployment. It can go stale,
produce a different result on another machine, and be forgotten at deployment
time — and each of those problems surfaces in operation, not while building.
Without it, the question "is the same thing running there as here?" is answered
by comparing files. And rolling back is copying back, with nothing having to be
regenerated.

**Obvious and rejected.** *Bundle and minify, "that is what one does"* — pays off
for large applications with many dependencies; for an application running off a
handful of files you buy milliseconds of load time with a toolchain that wants
maintaining. *A build step "only" for the third-party libraries* — then it
exists, so the target system will need it eventually.

**Symptom of violation.** Somebody asks "which version is running?", and the
answer requires executing a tool.

### F2 · The file is delivered unauthenticated — and therefore carries no data

**What holds.** The document is retrievable without any credential. Precisely
therefore it contains **exactly two things: form and hydration logic** — the
empty structure with its labels, and the code that fills it at runtime.
**Everything of substance arrives through the API**: no names, no lists, no
prefilled values, no keys, not a single record. Every request yields the same
bytes.

**The dividing line is not "sounds domain-specific" but "changes with the
data".** A label such as "storage location" is form, although it comes from the
domain vocabulary — it reads the same in every request. A concrete storage
location is content. The test in case of doubt: *would something else stand here
for a different dataset?* If so, it does not belong in the document but in a
response.

**Why.** This is not a sacrifice but a condition. The application is what
**obtains** a credential — you cannot demand one for loading it without going in
a circle. Since everyone gets it, everything in it is published. The gain: **the
frontend stops being a trust boundary.** There is no check that must be right at
delivery time and no request that shows more than another. What needs protecting
sits behind the API and is checked there — in one place instead of two.

**Obvious and rejected.** *Fill the first screen server-side so it is not empty*
— that turns delivery back into an authorization question, and the check exists
in two places. *Put the file behind a simple access barrier* — it would have to
apply before the invitation flow, hence before everything that creates a
credential; that is the circle above. *Embed "just" the configuration* (API
address, application name) — as soon as something is embedded, the question
becomes what else, and the line moves with every wish. Configuration belongs in a
separate, equally public file — then the rule is unambiguous instead of
negotiable.

**Symptom of violation.** Two requests for the document yield different bytes.

### F3 · No application code runs in the delivery path

**What holds.** The document comes from a file server. No template is filled, no
script runs server-side, nothing is assembled.

**Why.** What is not executed cannot be executed wrongly. With the server-side
template a whole class of faults disappears — embedded foreign content, wrongly
escaped values, template injection — and not because someone avoids it carefully,
but because the place does not exist. That is the difference between a measure
that must be maintained and a property of the construction.

It is also the precondition for delivery being able to move elsewhere: a file
server, an object store, a CDN — for an artefact without application code these
are interchangeable options.

**Obvious and rejected.** *A small template, "just for the title"* — it creates
exactly the place at issue, and the next requirement puts something substantive
into it. *Server-side pre-rendering for search engines* — right if the page is
meant to be found publicly; for an application behind a credential it is delivery
to nobody.

**Symptom of violation.** The document contains a place where a value is
inserted.

### F4 · The address names a state of the client, not a resource of the server

**What holds.** The server knows exactly **one** entry point and always delivers
the same document. Which view the application shows is the client's business; the
address expresses that state but names nothing the server could resolve.

**Why.** This is the actual shift of this cut, and it is often missed: in a
server-side application the address **is** a resource — the server looks it up
and answers. Here there is nothing to look up; there is a document, and within it
a state. Whoever keeps thinking the address is a resource inevitably builds a
server that has to pretend it is — and that is exactly where the configuration
comes from that this concept means to avoid (F5).

The consequence is also what **cannot** be done: there is no address under which
the server could deliver a partial view, no server-side pagination, no partial
page for a request without JavaScript.

**Obvious and rejected.** *Carry the views as real paths and have the server
resolve them* — then it is no longer a single-file application but a server-side
one with an addition. *One HTML file per view* — solves the address problem and
brings back everything F1 avoids: shared parts must be kept in sync, so you need
a build step or copies.

**Symptom of violation.** The server has to know which views the application has.

### F5 · The state sits in the fragment, so the server need not be bent

**What holds.** The state from F4 sits **after the `#`** (`…/#item/7`). The
application reads it at startup and listens for its change; it does **not** use
the History API with path-like addresses.

**Why.** The difference is what the server gets to see:

| | path-like address (`/item/7`) | fragment (`#item/7`) |
|---|---|---|
| What the server is asked for | exactly that path | the entry point only |
| What it needs for that | a catch-all rule mapping **every** path onto the same document | nothing |
| What gets deployed | artefact **and** server configuration | artefact |
| Switching while running | change via History API without reloading | change **without any request**, same context |

The core is the third row. **With path-like addresses, a server configuration
belongs to the artefact, and it must match the application's views** — it is set
up anew on every target system, it goes stale silently when someone adds a view,
and it is written differently on different servers. Deployment is then no longer
a copy operation, and the core of this concept is gone.

The fourth row is the practical bonus: if only the fragment changes, **the
browser loads nothing at all**. Same document, same JavaScript context, same
state in memory — the application merely re-renders. Browser back and forward
keep working either way.

**Obvious and rejected.** *History API, because the addresses are nicer* — the
only reason, and a real one; it costs the freedom from configuration. Whoever has
an environment where a catch-all rule is maintained anyway may choose it. *Offer
both* — then both routes must be tested, and the application carries two address
schemes for the same thing. *No routing at all, everything on one screen* —
carries until the second view, after which nothing can be linked.

**Symptom of violation.** After deploying to a new target system, every address
except the start page answers `404`.

### F6 · Third-party means are vendored, not linked

**What holds.** Third-party libraries sit as files in the artefact, versioned. No
script and no stylesheet is loaded from a foreign server.

**Why.** A linked foreign script runs **in your own origin** and can do
everything your own code can. Whoever serves it thereby decides the content of
your application at any time — without anyone noticing. This is not a question of
trusting a particular address but of how many parties have a say in what gets
delivered; the right answer is one. On top of that: a vendored means also works
when the foreign server is unreachable, and it still works in ten years.

**Obvious and rejected.** *Link with an integrity check* — rules out substitution
but not outage, and ties your availability to someone else's. *Link, "it is the
common address after all"* — popularity is not protection but an incentive for
whoever takes it over.

**Symptom of violation.** The application is broken in aeroplane mode or behind a
restrictive firewall.

### F7 · Files and binary data bypass the API envelope — recommendation

**What holds.** Anything that does not fit through the API protocol's envelope —
images, attachments, archives — should get a route of its own: a separate intake
for uploading and direct delivery by the file server for downloading. **How**
exactly depends on the chosen API technology and is to be decided there.

**Why.** A protocol with a fixed envelope — of whatever kind — transports bytes
only encoded, and every encoding costs volume and forces the data into memory in
full. This is not an implementation detail: it decides whether memory demand
grows with the **number of requests** or with the **size of the payload**. Whoever
notices only when the first large file arrives has to change the interface.

This is a **recommendation**, not a requirement: this cut does not commit to an
API technology. Whoever picks one should answer this question before the first
file, not after.

**Obvious and rejected.** *Send it encoded for now and change it later* — the
change hits client and server at once and is therefore the most expensive kind.
*Serve files through the application to check permissions* — then application
code runs for every thumbnail, and F3 is abandoned for the bulk of the traffic.

**Symptom of violation.** The server's memory demand depends on payload size
rather than on the number of requests.

### F8 · Delivery and API under one origin — recommendation

**What holds.** Static artefact and API should sit under the same origin. **How**
that is achieved is open: through directories below one base, through an upstream
intermediary that joins both, or through a CDN with path routing.

**Why.** Two origins mean CORS headers, a preflight before every call with JSON
content, and cookies that count as third-party and vanish depending on browser
settings. None of it is unsolvable; all of it is for nothing when it can be
avoided. A credential carried as a cookie works without special handling only
under this condition.

This too is a **recommendation**: separation can be right when frontend and API
are operated separately or scale differently. Then the effort is bought
deliberately instead of inherited by accident.

**Obvious and rejected.** *A separate subdomain for the API, "that is tidier"* —
proper and customary, costs three construction sites here for no return. *The
artefact from a CDN, the API from your own server, without joining them* —
precisely the case where the three construction sites appear; with path routing
the same benefit is available without them.

**Load-bearing structure.** **F2 carries the security half, F1 the deployment
half** — and they meet: because anyone may have the file, nothing may be in it;
because nothing is in it, deploying is a copy operation. If F2 falls, the
frontend becomes a trust boundary again and F3 loses its purpose. **F5 rests on
F4**, not the other way round: F4 says the address is a client state, F5 draws
the consequence for how it is written. Whoever drops F5 keeps F4 — but pays with
a server configuration and thereby loses the core of F1. F6 stands independently
and is adoptable on its own. **F7 and F8 are recommendations**, not requirements:
they concern the interface this cut does not commit to. **Whoever takes only one
rule takes F2.**

## 5. Security considerations

| Threat | Typical course | Countermeasure |
|---|---|---|
| **The artefact gives something away** | Someone embeds "just" the API address, then an application name, then a picklist of locations because "it hardly ever changes". The file is public, so the list is too | F2 as an **unambiguous** rule: only form and hydration logic, everything substantive through the API. The test — *would something else stand here for a different dataset?* — also settles the borderline cases that otherwise get negotiated |
| **Server-side inserted values** | A template fills a title from a parameter; the parameter contains markup | Eliminated by F3 — there is no place where anything is inserted. That is the difference between a maintained measure and a property of the construction |
| **Foreign script in your own origin** | A linked script is swapped out on the foreign server. It runs with all the rights of your own code, reads the state, and can issue calls with the user's credential | F6: vendor rather than link. An integrity check rules out substitution, not outage |
| **Everything hangs on the API** | Since the frontend checks nothing, the API is the only instance that does. A fault there is a fault everywhere | That is intended and the gain of F2: **one** checkpoint instead of two that drift apart. It must therefore actually be complete — no call may rely on "the frontend does not offer that anyway" |
| **Hiding instead of protecting** | A view is hidden in the client because the user should not see it — while its calls remain open | Only the API decides. What is hidden in the client is **not** protected; the artefact is public and contains the hidden branch verbatim |
| **State in the address is visible** | The fragment sits in the browser history and on screen. Whoever uses it for identifiers puts them on display | The fragment reaches no server (F5) but is locally visible. It is fit for views and identifiers that are shown anyway — not as a hiding place |

**What the concept does not protect against:** it does not replace an
authorization check — it moves it entirely into the API. It does not protect
against flaws in your own JavaScript, nor against a compromised delivery
location: whoever can swap the artefact swaps the application.

## 6. What it costs

- **The first screen is empty** until the first response arrives. Without
  server-side preparation there is nothing to show; a visible loading state is
  mandatory, not polish.
- **Nothing works without JavaScript.** There is no subset of the application
  that functions without it.
- **Search engines see one page.** For an application behind a credential that is
  inconsequential; for anything public it is disqualifying.
- **The addresses are uglier.** The `#` is visible and cannot be argued away.
- **Vendored means are maintained by you.** A vendored library gets no update for
  free; whoever wants to close a vulnerability must notice it and swap the file.
- **Everything at runtime means more round trips.** What a server-side
  application delivers in one response is here a document plus at least one call.

## 7. Scope and limits

**Preconditions.** A place that serves files — deployment needs no more than
that. An API carrying the application logic. Clients with JavaScript. An
application size that does without bundling: a manageable number of files and
dependencies.

**Tipping conditions.** The cut becomes wrong when:

- the application is meant to be **publicly discoverable** — search engines and
  link previews need server-generated markup;
- the application grows so large that **loading it all at once** becomes
  noticeable — then you need loading in parts, and with it usually a build step
  after all;
- **access without JavaScript** is required, for accessibility or policy reasons;
- the **first paint measurably counts** — retail, advertising goals. An empty
  first screen is a business problem there, not a blemish.

**Not solved.** This dossier says nothing about **what the API looks like** that
the application talks to — only that there is one and that everything substantive
goes through it. It says nothing about the **access model**: how a credential
comes into being, how it is transported and checked, how an invitation works. It
says nothing about the **internal structure of the application** — state keeping,
rendering, input validation. And it says nothing about **where it is deployed to**;
it merely makes deployment so undemanding that the question can stay open.

## 8. Track record

> **Example** (an inventory system; names and values invented). What gets
> deployed is a directory:
>
> ```
> index.html          ← the document, the same bytes for everyone
> app.js              ← the application, unmodified from the repository
> app.css
> vendor/qr.js        ← vendored third-party library (F6)
> ```
>
> A label on a box carries the address `https://…/#7-m3xq8f`. Tapping it triggers
> exactly one request to the server: `GET /`. The server delivers `index.html` —
> the same file as for every other request, without knowing that box 7 is at
> issue. **It has never seen the fragment** (F5).
>
> Only in the browser does the application read `#7-m3xq8f`, recognize the desired
> view from it, and fetch the content through the API. If the user then taps a
> second box, only the fragment changes: **no request to the server, no reload**,
> same document, same context — the application merely re-renders.
>
> What was deployed to make that possible: **the four files above.** No rewrite
> rule, no build step, no template. Getting onto a second target system means
> copying the directory there.

**Reference implementation: omitted, because none exists yet (status: Draft).**
The cut is fully decided and specified, but the running application it comes from
does not implement it — it predates this decision and serves several HTML files.
The statements in this dossier are therefore **thought through, not proven**; in
particular the claim in F1 that nothing is missing without a build step is so far
an argument and not experience.

## 9. Open questions

**Q1 · Where is the size limit of F1?** "No build step" holds for a handful of
files. When does it tip — at twenty modules, at the first dependency available
only as a package? What is wanted is a criterion applicable **beforehand**, not
the insight after the first slow load.

**Q2 · How does one keep vendored third-party means current?** F6 accepts the
maintenance but does not say how you learn of a vulnerability when no package
manager reads along. Recording the version in the artefact is the minimum — is
that enough?

**Q3 · Is the empty first screen really inconsequential?** For an application
behind a credential it is readily claimed that only usability counts. Beyond what
waiting time does that stop being true, and is a skeleton drawn from the artefact
(without data) still compatible with F2?

**Q4 · Does F5 hold for deep states too?** A fragment is a flat string. For "which
view, which object" that suffices; for filters, sorting, pagination and expanded
sections it quickly becomes a small format of its own. From when on is that a
fault rather than a state?

## 10. Sources & related work

- **Fragment identifiers in addresses**, RFC 3986 section 3.5 — the provision
  that the fragment part is **not** transmitted to the server, on which F5 rests:
  <https://www.rfc-editor.org/rfc/rfc3986#section-3.5> (retrieved 2026-08-15).
- **History API**, the counter-option in F5, including the note that the
  addresses it produces do reach the server on the next call:
  <https://developer.mozilla.org/en-US/docs/Web/API/History_API> (retrieved
  2026-08-15).
- **Same-origin policy and CORS preflight**, the construction sites F8 avoids:
  <https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS> (retrieved
  2026-08-15).
- **Deepening, not a prerequisite — the other side:** the sister dossier
  `single-endpoint-json-rpc`
  (<https://github.com/schlingensiepen/proposals/tree/main/single-endpoint-json-rpc>)
  describes an API fitting this cut. **It overlaps with this dossier in two
  places** — the exception for files (here F7) and the single origin (here F8).
  There they are **requirements**, because a concrete stack is described; here
  they are **recommendations**, because this cut commits to no API technology.
  Both are complete on their own.
- **Deepening, not a prerequisite — access:** how a credential comes into being,
  how an invitation works, and why an invitation link can carry its secret in the
  fragment without landing in an access log is the subject of a dossier of its
  own on the access model. For this cut it suffices that such a link **keeps
  working**, because F5 evaluates the fragment anyway.
