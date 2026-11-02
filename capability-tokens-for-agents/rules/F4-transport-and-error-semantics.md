---
type: Proposal Rule
description: Tokens travel only in the bearer header, logs carry only the short ID, unknown tokens get 401, and someone else's resources answer identically to nonexistent ones.
number: F4
binding: must
role: load-bearing
applies_to: [operator]
---

# F4 · Transport and error semantics: no oracles, no leak surfaces

**Applies to:** Operator.

**What holds.** Tokens travel exclusively in the bearer header — never in the URL, never in the payload. The service's logs contain only the short ID, never the token. And the error semantics gives no oracle: unknown tokens receive 401; someone else's resources answer **identically** to nonexistent ones (404). Anyone who guesses learns nothing — not even that there is anything to guess.

**Why.** URLs end up in proxy logs, browser histories, and application logs — the header path shuts these leak channels out by construction rather than by policy. And the existence of a resource is itself information: a 403 answer ("exists, is not yours") betrays the map of the service to anyone who is searching.

**Considered and rejected.** *The token as a URL parameter "for easy integration"* — then every place that stores URLs stores secrets. *Distinguishable errors "for better diagnostics"* — the diagnosis belongs in the teaching error message for the legitimate, authenticated caller, not in the answer to the anonymous guesser.

**How you notice it is violated.** A token turns up in an access log — or a systematic guesser can read off from the answers which resources exist.
