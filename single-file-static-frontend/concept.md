---
type: Proposal Concept
description: The problem of a frontend made of three matching parts, the concept of one static, data-free file whose address names a client state, its load-bearing structure, and the procedures for setting up and for reviewing.
---

# The concept

## Problem

"I want an application in the browser, and I want to deploy it without thinking every time. Today my frontend consists of a build result, a few templates the server fills, and a rewrite rule so the addresses look nice. Three things that must match — and when I add a view, I have to remember to update the server configuration. On a second target system I start over."

Two questions hide in that, and they belong together:

- **What exactly gets deployed?** As long as the answer is "files plus a configuration plus a build step", deployment is a procedure with ordering, states and failure modes — not copying.
- **What is in it that not everyone may see?** As soon as the server fills templates with user data, the delivered page differs per request, and who gets it is decided by a check somebody must have built correctly.

## The concept

The frontend is **one** HTML file, and it carries its own code. The third-party libraries it needs are beside it as files, versioned and unmodified; there is no build step turning sources into a result. This file is delivered **without authentication**, since it is the very thing that obtains a credential in the first place. So nothing may be in it that not everyone may see. **The markup carries only form and hydration logic** — the empty structure and the code that fills it; everything of substance arrives at runtime through an API. **No application code runs in the delivery path** — no server rendering, no template that inserts anything; a file server suffices.

This shifts what an address means: it no longer names a **resource on the server** but a **state of the client**. The server has exactly one entry point and always delivers the same document. The desired state is therefore in the **fragment** — after the `#` — which the browser never asks the server about. Third-party libraries are vendored rather than loaded from external servers. And whatever does not fit through a JSON interface — images, archives — gets a route of its own.

**Load-bearing structure.** **F2 carries the security half, F1 the deployment half** — and they meet: because anyone may have the file, nothing may be in it; because nothing is in it, deploying is a copy operation. If F2 falls, the frontend becomes a trust boundary again and F3 loses its purpose. **F5 rests on F4**, not the other way round: F4 says the address is a client state, F5 draws the consequence for how it is written. If you drop F5, you keep F4 — but you pay with a server configuration and thereby lose the core of F1. F6 stands independently and is adoptable on its own. **F7 and F8 are recommendations**, not requirements: they concern the interface this design does not commit to. **If you take only one rule, take F2.**

## The procedure

The rules say what must hold. This section says in which order one proceeds, once for a new application and once for reviewing an existing one.

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
