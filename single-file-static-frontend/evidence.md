---
type: Proposal Evidence
description: An invented walk-through of an inventory application deployed as two files, and the honest state, that no reference implementation exists yet and the design is thought through, not proven.
sources: []
---

# Evidence

## Origin

The design comes from a running application that predates this decision and still serves several HTML files (see the reference implementation below). The core of the design is simple deployment while keeping structural security; both halves are the same thing seen from two sides. A hosting restriction such as "no URL rewriting" is in this proposal explicitly an example, not the cause.

## A walk-through (invented)

> **Example** (an inventory system; names and values invented). What gets deployed is a directory:
>
> ```
> index.html          ← the document with its own code, the same bytes for everyone
> vendor/qr.js        ← vendored third-party library (F6)
> ```
>
> A label on a box carries the address `https://…/#7-m3xq8f`. Tapping it triggers exactly one request to the server: `GET /`. The server delivers `index.html` — the same file as for every other request, without knowing that box 7 is at issue. **It has never seen the fragment** (F5).
>
> Only in the browser does the application read `#7-m3xq8f`, recognize the desired view from it, and fetch the content through the API. If the user then taps a second box, only the fragment changes: **no request to the server, no reload**, same document, same context — the application merely re-renders.
>
> What was deployed to make that possible: **the two files above.** No rewrite rule, no build step, no template. Getting onto a second target system means copying the directory there.

## Reference implementation

**Reference implementation: omitted, because none exists yet (status: draft).** The design is fully decided and specified, but the running application it comes from does not implement it — it predates this decision and serves several HTML files. The statements in this proposal are therefore **thought through, not proven**; in particular the claim in F1 that nothing is missing without a build step is so far an argument and not experience.
