---
type: Proposal Scope
description: Security considerations, what the cut costs, where it applies, when the answer flips, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

| Threat | Typical course | Countermeasure |
|---|---|---|
| **The artifact gives something away** | Someone embeds "just" the API address, then an application name, then a picklist of locations because "it hardly ever changes". The file is public, so the list is too | F2 as an **unambiguous** rule: only form and hydration logic, everything substantive through the API. The test — *would something else stand here for a different dataset?* — also settles the borderline cases that otherwise get negotiated |
| **Server-side inserted values** | A template fills a title from a parameter; the parameter contains markup | Eliminated by F3 — there is no place where anything is inserted. That is the difference between a maintained measure and a property of the construction |
| **Foreign script in your own origin** | A linked script is swapped out on the foreign server. It runs with all the rights of your own code, reads the state, and can issue calls with the user's credential | F6: vendor rather than link. An integrity check rules out substitution, not outage |
| **Everything hangs on the API** | Since the frontend checks nothing, the API is the only instance that does. A fault there is a fault everywhere | That is intended and the gain of F2: **one** checkpoint instead of two that drift apart. It must therefore actually be complete — no call may rely on "the frontend does not offer that anyway" |
| **Hiding instead of protecting** | A view is hidden in the client because the user should not see it — while its calls remain open | Only the API decides. What is hidden in the client is **not** protected; the artifact is public and contains the hidden branch verbatim |
| **State in the address is visible** | The fragment sits in the browser history and on screen. Whoever uses it for identifiers puts them on display | The fragment reaches no server (F5) but is locally visible. It is fit for views and identifiers that are shown anyway — not as a hiding place |

**What the concept does not protect against:** it does not replace an authorization check — it moves it entirely into the API. It does not protect against flaws in your own JavaScript, nor against a compromised delivery location: whoever can swap the artifact swaps the application.

## What it costs

- **The first screen is empty** until the first response arrives. Without server-side preparation there is nothing to show; a visible loading state is mandatory, not polish.
- **Nothing works without JavaScript.** There is no subset of the application that functions without it.
- **Search engines see one page.** For an application behind a credential that is inconsequential; for anything public it is disqualifying.
- **The addresses are uglier.** The `#` is visible and cannot be argued away.
- **Vendored assets are maintained by you.** A vendored library gets no update for free; whoever wants to close a vulnerability must notice it and swap the file.
- **Everything at runtime means more round trips.** What a server-side application delivers in one response is here a document plus at least one call.

## Where it applies

### Preconditions

A place that serves files — deployment needs no more than that. An API carrying the application logic. Clients with JavaScript. An application size that does without bundling: a manageable number of files and dependencies.

### When the answer flips

The cut becomes wrong when:

- the application is meant to be **publicly discoverable** — search engines and link previews need server-generated markup;
- the application grows so large that **loading it all at once** becomes noticeable — then you need loading in parts, and with it usually a build step after all;
- **access without JavaScript** is required, for accessibility or policy reasons;
- the **first paint measurably counts** — retail, advertising goals. An empty first screen is a business problem there, not a blemish.

### What it does not solve

This proposal says nothing about **what the API looks like** that the application talks to — only that there is one and that everything substantive goes through it. It says nothing about the **access model**: how a credential comes into being, how it is transported and checked, how an invitation works. It says nothing about the **internal structure of the application** — state keeping, rendering, input validation. And it says nothing about **where it is deployed to**; it merely makes deployment so undemanding that the question can stay open.

### Not yet settled

* **Where the size limit of F1 lies.** "No build step" holds for a handful of files. When does it flip — at twenty modules, at the first dependency available only as a package? What is wanted is a criterion applicable **beforehand**, not the insight after the first slow load.
* **How to keep vendored third-party assets current.** F6 accepts the maintenance but does not say how you learn of a vulnerability when no package manager reads along. Recording the version in the artifact is the minimum, and the agent guide makes it a duty; whether that is enough is open.
* **Whether the empty first screen is really inconsequential.** For an application behind a credential it is readily claimed that only usability counts. Beyond what waiting time does that stop being true, and is a skeleton drawn from the artifact (without data) still compatible with F2?
* **Whether F5 holds for deep states too.** A fragment is a flat string. For "which view, which object" that suffices; for filters, sorting, pagination and expanded sections it quickly becomes a small format of its own. From when on is that a fault rather than a state?
