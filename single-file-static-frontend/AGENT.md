---
type: Proposal Agent Guide
name: single-file-static-frontend
description: Self-contained guide for an agent building or reviewing a browser application that deploys as a plain copy of files, a single static HTML document plus vendored assets with no build step and no server-side rendering; when to apply, rules, recommendations, procedure, acceptance checklist, common mistakes.
---

# Agent guide

## Purpose

Two sentences carry everything below: **because anyone may have the file, nothing may be in it — and because nothing is in it, deploying is a copy operation.** The delivered markup holds only form and hydration logic; every piece of content arrives at runtime through an API.

This guide is complete on its own. Every rule, step and checklist item is written out in full here; rule numbers in parentheses are provenance only, and you never need to open a rule file to act correctly. The reasoning, the costs and the limits are in the concept, the rules and the scope of this bundle.

Apply this guide when:

- you are building a browser application whose deployment should be copying files, with no build step and no server-side rendering;
- you are reviewing such an application, or converting a server-rendered one to this design;
- you are choosing between fragment routing and the History API;
- you are deciding what may be embedded in the delivered HTML.

**Do not apply** when any of these hold: the application must be publicly discoverable (search engines and link previews need server-generated markup); it must work without JavaScript; it grows large enough that loading everything at once is noticeable; or first paint is a measured business goal. Each of these makes the rules below wrong rather than merely inconvenient.

## Procedure

**Setting up the artifact:**

1. Create one HTML document containing structure, static labels and the application's own code (the hydration logic) only.
2. Put the vendored libraries beside it as plain files; record each library's version.
3. Put configuration into a separate public file the application fetches at startup.
4. Implement hydration: at startup, read the fragment, derive the view, call the API, fill the structure.
5. Register a listener for fragment changes that re-renders without reloading.

**Reviewing an existing application against this design:**

1. Request the document twice, unauthenticated, and compare the bytes. They must be identical.
2. Grep the delivered markup for anything that varies per dataset — names, identifiers, picklists.
3. Confirm no server-side template engine touches the document.
4. Confirm the server needs no rule beyond serving the entry point; deploy to a clean target and check that every view is reachable.
5. Grep for `<script src="http` and `<link href="http` pointing at external hosts.
6. Check that every view hidden in the client is also refused by the API.

## Rules (MUST / NEVER)

MUST:

1. Deploy exactly what is versioned in the repository: one HTML document with its own code and the vendored files beside it. No bundling, no transpiling, no minifying, no generated output. (F1)
2. Serve the document without requiring any credential. It is the thing that obtains a credential, so demanding one to load it is circular. (F2)
3. Put only two things in the delivered markup: **form** — structure, layout and static labels — and **hydration logic**, the code that fills that structure at runtime. Everything else arrives through the API. (F2)
4. Decide borderline cases with this test: *would something else stand here for a different dataset?* If yes, it is content and belongs in a response, not in the document. A label like "storage location" is form; a concrete storage location is content. (F2)
5. Keep configuration — API address, application name — in a separate, equally public file, never embedded in the document. Otherwise the boundary shifts with every new request for an exception. (F2)
6. Serve the document from a file server with no application code in the delivery path: no template filled, no script run server-side, nothing assembled. (F3)
7. Treat the address as a **state of the client**, never as a resource the server resolves. The server knows one entry point and always returns the same document. (F4)
8. Write that state into the **fragment** (`…/#item/7`). Read it at startup and listen for its change. (F5)
9. Vendor every third-party script and stylesheet as a file inside the artifact, versioned. (F6)
10. Show a visible loading state. Without server-side preparation the first screen is empty until the first response arrives; this is mandatory, not polish. (scope: what it costs)
11. Record the version of every vendored library in the artifact, so someone can tell what is running and whether it needs replacing. (F6)

NEVER:

- Never embed substantive content in the delivered markup — no names, no lists, no prefilled values, no keys, not a single record. Two requests for the document must yield identical bytes. (F2)
- Never fill the first screen server-side "so it is not empty". That turns delivery back into an authorization question and puts the check in two places. (F2, F3)
- Never insert a value into the document server-side, not even "just the title". That creates exactly the place this design removes. (F3)
- Never use the History API with path-like addresses. Every incoming link and every reload then asks the server for that path, so the server needs a catch-all rule — and a server configuration that must track the application's views becomes part of what you deploy. (F5)
- Never require the server to know which views the application has. (F4)
- Never load a script or stylesheet from an external server. It runs in your own origin with all the rights of your own code, and anyone who serves it decides your application's content at any time. An integrity check rules out substitution but not outage. (F6)
- Never treat hiding a view in the client as protection. The artifact is public and contains the hidden branch verbatim; only the API decides. (F2)
- Never put anything in the fragment that should stay unseen. It reaches no server, but it is in the browser history and on screen. (F5)

## Recommendations (SHOULD)

These concern the interface, which this guide does not fix. Decide them explicitly rather than by default.

- Route files and binary data **around** the API envelope: a separate intake for uploads, direct delivery by the file server for downloads. Any fixed envelope transports bytes only encoded, which forces the whole payload into memory — memory demand must scale with the number of requests, not with payload size. Decide this before the first large file; changing it later hits client and server at once. (F7)
- Put the static artifact and the API under the **same origin** — via directories below one base, an upstream intermediary, or a CDN with path routing. Two origins mean CORS headers, a preflight before every JSON call, and cookies counted as third-party. Separate origins are right only when frontend and API are operated separately or scale differently. (F8)

## Acceptance checklist

- [ ] Is what gets deployed identical to what is versioned, with no build step? (F1)
- [ ] Is the document retrievable without any credential? (F2)
- [ ] Do two unauthenticated requests yield byte-identical documents? (F2)
- [ ] Does the markup contain only form and hydration logic, with no value that would differ for another dataset? (F2)
- [ ] Is configuration in a separate file rather than embedded? (F2)
- [ ] Does no application code run in the delivery path? (F3)
- [ ] Does the server need to know nothing about the application's views? (F4)
- [ ] Is the state in the fragment, with no catch-all rule required? (F5)
- [ ] Does changing only the fragment cause no request and no reload? (F5)
- [ ] Is every third-party script and stylesheet vendored, with its version recorded? (F6)
- [ ] Is there a visible loading state for the empty first screen? (scope: what it costs)
- [ ] Is every client-side hidden view also refused by the API? (F2)
- [ ] Has the route for files and binary data been decided explicitly? (F7)
- [ ] Do artifact and API share one origin, or is the separation deliberate? (F8)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Picklist embedded in the HTML "because it hardly changes" | Fetch it through the API; the document is public, so the list would be too |
| API address hardcoded in the document | Move it to a separate public configuration file |
| History API chosen "because the addresses are nicer" | Use the fragment, or accept that a server configuration is now part of every deployment |
| Server rewrite rule added to make deep links work | Remove the rule and move the state into the fragment; the artifact must carry its own routing |
| Third-party library loaded from a public CDN | Vendor the file and record its version |
| Menu entry hidden in the client for non-admins | Also refuse the corresponding calls in the API; hiding is not protecting |
| Title rendered server-side "just this once" | Render it in the client; one insertion point is one too many |
| Blank screen with no indication during startup | Add a visible loading state; there is no server-side first paint to fall back on |
| Session identifier written into the fragment | Keep it out; the fragment is visible on screen and in history |

## Self-check

Before you report completion: can you show two byte-identical unauthenticated responses for the document, name the one entry point, list every vendored library with its version, and state your explicit decisions on the route for files and on the origin? If any of these is missing, you are not done. If a rule seems wrong for your case, read its file in `rules/` and the scope before deviating, and record the deviation.
