---
type: Proposal Rule
description: Frontend files and API live under the same origin, separated by directories below one base.
number: F10
binding: must
role: independent
applies_to: [builder]
---

# F10 · Static delivery and API are one origin to the outside

**Applies to:** builder.

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

**Considered and rejected.** *API on its own subdomain* — proper and customary, but
here it costs exactly the three construction sites above for no return; a
separate subdomain pays off when API and frontend scale separately or are run by
different teams. *Static files from a CDN, API from your own server* — sensible
for large, publicly readable content, and then to be solved via a shared origin
with path routing; here everything is behind a credential, so a CDN would have
nothing it may serve.

**How you notice it is violated.** An `OPTIONS` precedes every call in the log — or the
session cookie disappears in a browser with strict defaults.
