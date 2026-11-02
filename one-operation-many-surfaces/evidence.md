---
type: Proposal Evidence
description: Where the concept has held; its origin in the author's second administration surface, what the reference implementation builds and evidences, what is only partly met, and the incident of the ten places that drifted apart.
sources:
  - id: imap-service
    url: https://github.com/schlingensiepen/imap-service
    title: The author's mail-filing service, repository imap-service (not publicly accessible)
---

# Evidence

## Origin

The two rules come from an implementation by the author: a second administration surface over HTTP beside the command line of his mail-filing service, decided and built in September 2026. They first stood as two rules of a proposal for that administration surface (September 2026) and were made a concept of their own because they hold for every service with more than one surface. The repository of the implementation is not publicly accessible.

## The reference implementation

**What is built.** **One operation per administrative action** in a module that knows neither HTTP nor process exit and raises error kinds of its own, with **one translation function** per pass-through (to status codes and a closed error format here, to five exit codes there) and **the same response models** for the machine-readable output of both surfaces. **Structural tests on both sides**: for the command line a whitelist of the permitted imports from its own service, a list of operation names that may appear nowhere, a ban on any file system access with exactly one named exception for reading the password file named at invocation, a ban on any HTTP server, and the assurance that every command calls an operation and everything it calls really is one; for the HTTP side the same list of operation names, a ban on reaching the storage directly, a ban on any path access derived from the standard library, and the assurance that exactly one module writes the store and that it is not a route.

**What it evidences.** Touched and in operation is F1 (one operation, two pass-throughs, one translation each, the same models). **Partly met** is F2: for the command line there is the whitelist of imports, the list of operation names, the file system ban with a named exception and the **positive** assurance that every command calls an operation and everything it calls is one. For the HTTP side there are only absences (no forbidden symbols, no access to the storage, no path access, exactly one module writes the store) but **not** the assurance that every handler calls an operation; a handler that did something itself without needing a forbidden symbol or the file system for it would not be noticed there (not yet settled in the scope). The red check is evidenced on both sides as a working rule of the author, not logged per assertion.

**Incident, stripped down.** The administrative operations existed twice, once per surface. A later comparison found ten places where they had drifted apart: one surface reported more on a failure than the other, one knew a server feature the other did not read, one turned every unknown status into a server error, one had one status line fewer. Each of the two foundations for the target system had to be hardened twice (a silent fallback to an unencrypted login and an ineffective timeout were each fixed twice), and after a review the gate of the user-facing surface got six new checks that the hand-copied version for the administration did not have, which is why a description passed there that kept advertising removed behavior. Price: a rebuild that touched user-facing surface, administration and deliverer at the same time, and afterwards all three through the check again. Since then: one operation, two pass-throughs, one structural test per side, one gate.

**Source.** The author's mail-filing service, repository `imap-service` [^imap-service].

[^imap-service]: https://github.com/schlingensiepen/imap-service (not publicly accessible)
