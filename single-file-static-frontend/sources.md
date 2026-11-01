---
type: Proposal Sources
description: What this proposal builds on; it imports no other concept; one proposal of the series is recommended together with it; three external sources.
sources:
  - id: rfc3986-fragment
    url: https://www.rfc-editor.org/rfc/rfc3986#section-3.5
    title: RFC 3986, section 3.5, Fragment
  - id: mdn-history-api
    url: https://developer.mozilla.org/en-US/docs/Web/API/History_API
    title: MDN Web Docs, History API
  - id: mdn-cors
    url: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
    title: MDN Web Docs, Cross-Origin Resource Sharing (CORS)
---

# Sources

## Imports

This proposal imports no other concept.

## Related

* `single-endpoint-json-rpc` (recommended-with): an API that fits this cut. The two overlap in two places, the exception for files (here F7) and the single origin (here F8). Here they are recommendations, because this cut commits to no API technology. Each proposal is complete on its own; further reading, not a prerequisite.
* The access model (recommended-with; further reading, not a prerequisite): how a credential comes into being, how an invitation works, and why an invitation link can carry its secret in the fragment without landing in an access log is the subject of a proposal of its own on the access model. For this cut it suffices that such a link **keeps working**, because F5 evaluates the fragment anyway.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* Fragment identifiers in addresses, RFC 3986 section 3.5 [^rfc3986-fragment]: the provision that the fragment part is **not** transmitted to the server, on which F5 rests (retrieved 2026-08-15).
* History API [^mdn-history-api]: the counter-option in F5, including the note that the addresses it produces do reach the server on the next call (retrieved 2026-08-15).
* Same-origin policy and CORS preflight [^mdn-cors]: the problems F8 avoids (retrieved 2026-08-15).

[^rfc3986-fragment]: https://www.rfc-editor.org/rfc/rfc3986#section-3.5
[^mdn-history-api]: https://developer.mozilla.org/en-US/docs/Web/API/History_API
[^mdn-cors]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
