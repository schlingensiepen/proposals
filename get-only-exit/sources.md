---
type: Proposal Sources
description: What this proposal builds on; one import, four proposals it stands in tension with, related proposals, and external sources checked on 2026-10-13.
sources:
  - id: rfc9110
    url: https://www.rfc-editor.org/rfc/rfc9110
    title: RFC 9110, HTTP Semantics
  - id: rfc6750
    url: https://www.rfc-editor.org/rfc/rfc6750
    title: "RFC 6750, The OAuth 2.0 Authorization Framework: Bearer Token Usage"
  - id: rfc3986
    url: https://www.rfc-editor.org/rfc/rfc3986
    title: "RFC 3986, Uniform Resource Identifier (URI): Generic Syntax"
  - id: rfc4648
    url: https://www.rfc-editor.org/rfc/rfc4648
    title: RFC 4648, The Base16, Base32, and Base64 Data Encodings
  - id: reference-implementation
    url: https://github.com/schlingensiepen/imap-service
    title: The author's mail-depositing service (not publicly accessible)
---

# Sources

## Imports

* `one-operation-many-surfaces`: every operation exists exactly once,
  and every surface is a pass-through to it. This concept takes that
  over for the second surface: it invokes the operations of the main
  surface and implements none of its own
  ([F1](./rules/F1-an-emergency-exit-not-a-second-api.md)).

## Tension

* `capability-tokens-for-agents`, `invitation-instead-of-login`,
  `single-endpoint-json-rpc`, `where-a-secret-comes-from` (tension-with):
  in tension.
  This concept puts the secret into the address and thereby explicitly
  suspends their rule that a secret never travels in an address; the
  price is named at the door ([F11](./rules/F11-the-price-named.md)).

## Related

* `self-describing-services` (recommended-with): deepening, not a prerequisite; the
  self-description whose weakest reader this surface serves.
* `service-as-single-artifact` (recommended-with): deepening, not a prerequisite; the
  question of cut beside this one.
* `admin-surface-over-http` (overlaps-with): named by that proposal from its side; see there.

All proposals live in the same repository; see its root README for the
register and the links. Which proposals build on this one is listed in
[`used-by.md`](./used-by.md).

## External sources

Checked against the published texts on 2026-10-13.

* RFC 9110, *HTTP Semantics* (R. Fielding, M. Nottingham, J. Reschke,
  June 2022, Internet Standard) [^rfc9110]: defines `GET` as a safe
  method whose repetition and prefetching are permitted (section 9.2.1),
  the reason why `GET`s that take effect are named as a violation in the
  scope and why [F6](./rules/F6-the-composition.md) allows exactly one
  call that takes effect; contains the section on disclosure of
  sensitive information in URIs (17.9), which marks the breach of
  [F11](./rules/F11-the-price-named.md) as such; and recommends
  supporting URIs of at least 8000 octets (4.1), the orientation for the
  scale in [F5](./rules/F5-the-upload-in-parts.md).
* RFC 6750, *The OAuth 2.0 Authorization Framework: Bearer Token Usage*
  (M. Jones, D. Hardt, October 2012, Standards Track) [^rfc6750]:
  section 2.3 describes the bearer token in the query part of an address
  as a method that "SHOULD NOT be used unless it is impossible to
  transport the access token in the Authorization request header field
  or the HTTP request entity-body", because of "the high likelihood that
  the URL containing the access token will be logged", and asks for
  `Cache-Control: no-store` on such requests. The same weighing F11
  makes, with the same result: possible, but only as an exception with a
  named price. That this concept takes the path instead of the query
  part is owed to the measured behavior of sandboxes
  ([F2](./rules/F2-everything-in-the-path-the-secret-first.md)), not to
  this source.
* RFC 3986, *Uniform Resource Identifier (URI): Generic Syntax*
  (T. Berners-Lee, R. Fielding, L. Masinter, January 2005, Internet
  Standard) [^rfc3986]: the terms path, path segment and query part,
  and percent encoding, on which [F3](./rules/F3-two-encoding-rules.md)
  sees the slash fail. An encoded slash is syntactically permitted and
  is nevertheless decoded before routing by many implementations; the
  concept relies on the measurement, not on the standard.
* RFC 4648, *The Base16, Base32, and Base64 Data Encodings*
  (S. Josefsson, October 2006, Standards Track) [^rfc4648]: section 5
  defines the URL- and filename-safe Base64 alphabet with `-` and `_`
  that F3 requires as base64url for free text and file parts. Section
  3.2 requires padding unless the referring specification explicitly
  states otherwise; this concept does state otherwise, which is why the
  padding may be omitted here.
* The author's mail-depositing service [^reference-implementation]: the
  reference implementation described in the evidence. The repository is
  not publicly accessible.

[^rfc9110]: https://www.rfc-editor.org/rfc/rfc9110
[^rfc6750]: https://www.rfc-editor.org/rfc/rfc6750
[^rfc3986]: https://www.rfc-editor.org/rfc/rfc3986
[^rfc4648]: https://www.rfc-editor.org/rfc/rfc4648
[^reference-implementation]: https://github.com/schlingensiepen/imap-service (not publicly accessible)

## Placement

This concept runs across a conviction that the rest of the series shares
and that RFC 9110 and RFC 6750 support: secrets travel in headers, and
`GET` has no effect. It does not contradict it; it describes how to
break it at exactly one named place, because otherwise a whole type of
tool stays excluded, and how to build the breach so that it stays a
breach: own prefix, a pass-through without a capability of its own, one
call that takes effect, a price at the door, a main surface that never
offers the way. The author calls the result "an anti-API". The name is
meant seriously and carries the costs openly; that is the only thing
that justifies such a surface.
