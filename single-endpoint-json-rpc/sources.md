---
type: Proposal Sources
description: What this proposal relates to; one related proposal, one tension, and external sources.
sources:
  - id: jsonrpc-spec
    url: https://www.jsonrpc.org/specification
    title: JSON-RPC 2.0 Specification
  - id: rfc9110
    url: https://www.rfc-editor.org/rfc/rfc9110.html
    title: RFC 9110, HTTP Semantics
  - id: mdn-cors
    url: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
    title: Same-origin policy and CORS preflight (MDN)
---

# Sources

## Imports

This concept imports no other proposal.

## Related

* `single-file-static-frontend` (recommended-with): recommended together with this concept; deepening, not a prerequisite. The two overlap in two places of this concept, the exception for files ([F8](./rules/F8-binary-data-leaves-the-envelope.md), [F9](./rules/F9-file-intake-carries-no-application-logic.md)) and the single origin ([F10](./rules/F10-static-delivery-and-api-are-one-origin.md)); here they are requirements.
* `invitation-instead-of-login` (recommended-with): named by that proposal from its side; see there.

## Tension

* `get-only-exit` (tension-with): in tension with [F7](./rules/F7-credentials-go-in-headers.md).

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* JSON-RPC 2.0 Specification [^jsonrpc-spec]: the envelope verbatim, including the reserved error codes and the optional batch and notification forms this concept excludes (retrieved 2026-08-14).
* RFC 9110, HTTP Semantics, section 15 [^rfc9110]: the meaning of the status codes whose semantics F5 carries into the error object (retrieved 2026-08-14).
* Same-origin policy and CORS preflight [^mdn-cors]: the three construction sites F10 avoids (retrieved 2026-08-14).

[^jsonrpc-spec]: https://www.jsonrpc.org/specification
[^rfc9110]: https://www.rfc-editor.org/rfc/rfc9110.html
[^mdn-cors]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS
