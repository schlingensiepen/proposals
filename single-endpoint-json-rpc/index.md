---
okf_version: "0.2"
---

# Start here

* [README](./README.md) - Identity card: question, thesis, version.
* [Concept](./concept.md) - The problem of an API on cheap shared hosting, the concept of one endpoint with one envelope and a separate path for bytes, and its load-bearing structure.
* [Terms](./terms.md) - The terms of this bundle in English and German, each with a short explanation.

# Rules

One answer each; give one up and you are no longer using the concept. Address and envelope F1 to F6, credentials F7, binary data F8, F9, F11 and F12, one origin F10.

* [F1 One address, reached through the default document](./rules/F1-one-address-through-the-default-document.md) - All application logic is behind exactly one address that points at a directory and is resolved to a script by the server's default document; no path behind it, no technology marker, no rewriting.
* [F2 An established envelope, not a homegrown one](./rules/F2-an-established-envelope.md) - The envelope is JSON-RPC 2.0, unchanged; no homegrown format and no extra fields at envelope level.
* [F3 No batch requests, no notifications](./rules/F3-no-batch-requests-no-notifications.md) - An array in the body and a request without an id are rejected, although the standard permits both.
* [F4 The HTTP status is 200; the outcome lives in the envelope](./rules/F4-the-http-status-is-200.md) - Whenever the request was readable as JSON-RPC the HTTP status is 200; only an unreadable body (400) and a method other than POST (405) differ, and both still carry a complete error object.
* [F5 The error has three fields with three jobs](./rules/F5-the-error-has-three-fields.md) - Every application error carries a number with HTTP status semantics, a machine-readable type and plain text for humans; all three are mandatory, while a protocol error carries only what the standard defines.
* [F6 Protocol errors and application errors are separate number spaces](./rules/F6-protocol-and-application-errors-are-separate.md) - The codes reserved by the standard stay exclusively with the protocol, application errors use only the HTTP-style codes, and no code appears in both roles.
* [F7 Credentials go in headers, never in the body, never in the address, with one named exception](./rules/F7-credentials-go-in-headers.md) - Every credential travels in an HTTP header, with one named exception, the redemption secret presented once in a request body; it arrives through the URL fragment and is exchanged for the session cookie, and five mechanisms are mandatory.
* [F8 Binary data leaves the envelope, in both directions](./rules/F8-binary-data-leaves-the-envelope.md) - Uploads use a separate multipart endpoint, downloads an unguessable address the web server serves directly, accepted only where a loss stays single.
* [F9 File intake carries no application logic](./rules/F9-file-intake-carries-no-application-logic.md) - The upload endpoint takes one file and returns one handle; the directly served upload area is protected by three barriers graded by obligation.
* [F10 Static delivery and API are one origin to the outside](./rules/F10-static-delivery-and-api-are-one-origin.md) - Frontend files and API live under the same origin, separated by directories below one base.
* [F11 The randomness is in the directory, not in the filename](./rules/F11-the-randomness-sits-in-the-directory.md) - Every file gets its own subdirectory named with a cryptographically random identifier and keeps its upload name inside it; in reading order it belongs between F8 and F9.
* [F12 Large compilations: commission, poll, fetch, terminate](./rules/F12-large-compilations.md) - Backups and exports go through methods on the endpoint; the path exists only after completion, and termination is mandatory for security-relevant data.

# Scope and evidence

* [Scope](./scope.md) - Security considerations, what it costs, preconditions, when this no longer holds, what it does not solve, what is not yet settled.
* [Evidence](./evidence.md) - A complete operation from start to finish, and the honest state of play: thought through, not yet proven.
* [Sources](./sources.md) - What this proposal relates to: one related proposal, one tension, and external sources.
* [Used by](./used-by.md) - Which proposals import or extend this one. Only needed when changing this concept.

# Adopting

* [AGENT](./AGENT.md) - Self-contained guide for an agent building or reviewing an API on this concept: procedures, rules, acceptance checklist, common mistakes, self-check.
* [Extensions](./extensions.md) - Proposals that extend this one.

# History

* [Update log](./log.md) - Dated changes to this bundle.
