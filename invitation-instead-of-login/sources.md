---
type: Proposal Sources
description: What this proposal builds on; related proposals of the series and external sources.
sources:
  - id: rfc6265
    url: https://www.rfc-editor.org/rfc/rfc6265.html
    title: HTTP State Management Mechanism (Cookies), RFC 6265
  - id: rfc3986-fragment
    url: https://www.rfc-editor.org/rfc/rfc3986#section-3.5
    title: Uniform Resource Identifier, RFC 3986, section 3.5 Fragment
  - id: owasp-session
    url: https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
    title: OWASP Session Management Cheat Sheet
---

# Sources

## Imports

None. This concept is complete on its own.

## Related

* `capability-tokens-for-agents` (overlaps-with): deepening, not a prerequisite; the capability side of F6 in detail. The boundary runs like this: there it is the capability side alone, with an **agent** as consumer that registers itself and receives its token over a second channel. Here it is the **boundary** between identity and capability in a system carrying both, and the machine consumer is a process the operator deploys; its key therefore comes into being in the deployment (F7) and not through self-registration. That is no contradiction but a different starting case: an agent turns up, a background process is put there.
* `single-endpoint-json-rpc` (recommended-with): deepening, not a prerequisite; the interface over which credentials travel. This concept is complete without it: it fixes **which** credentials exist and **how they come into being**, not over which interface they travel.
* `get-only-exit` (tension-with): in tension at F9.
* `push-deployment-that-keeps-its-secrets` (recommended-with): named by that proposal from its side; see there.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* HTTP State Management (Cookies), RFC 6265 and its successor draft [^rfc6265]: the provisions on `HttpOnly`, `Secure`, `SameSite` and the effective maximum lifetime of 400 days, where the longevity of credentials meets its limit (F4). Retrieved 2026-08-15.
* Fragment identifiers in addresses, RFC 3986 section 3.5 [^rfc3986-fragment]: the provision that the fragment part is not transmitted to the server, on which F9 rests. Retrieved 2026-08-15.
* OWASP Session Management Cheat Sheet [^owasp-session]: for placing the deviations from the usual sign-in solution taken here. Retrieved 2026-08-15.

[^rfc6265]: https://www.rfc-editor.org/rfc/rfc6265.html
[^rfc3986-fragment]: https://www.rfc-editor.org/rfc/rfc3986#section-3.5
[^owasp-session]: https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
