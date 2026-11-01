---
type: Proposal Rule
description: Static artifact and API should sit under the same origin, through directories below one base, an upstream intermediary or a CDN with path routing; separation is right only when bought deliberately.
number: F8
binding: should
role: independent
applies_to: [builder, operator]
---

# F8 · Delivery and API under one origin — recommendation

**Applies to:** builder, operator.

**What holds.** Static artifact and API should sit under the same origin. **How** that is achieved is open: through directories below one base, through an upstream intermediary that joins both, or through a CDN with path routing.

**Why.** Two origins mean CORS headers, a preflight before every call with JSON content, and cookies that count as third-party and vanish depending on browser settings. None of it is unsolvable; all of it is for nothing when it can be avoided. A credential carried as a cookie works without special handling only under this condition.

This too is a **recommendation**: separation can be right when frontend and API are operated separately or scale differently. Then the effort is bought deliberately instead of inherited by accident.

**Obvious and rejected.** *A separate subdomain for the API, "that is tidier"* — proper and customary, but here it costs these three problems for no return. *The artifact from a CDN, the API from your own server, without joining them* — precisely the case where the three problems appear; with path routing the same benefit is available without them.

**How you notice it is violated.** A preflight request precedes every call in the access log — or the credential disappears in a browser with strict default settings.
