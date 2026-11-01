---
type: Proposal Rule
description: The client state sits after the `#`; the application reads it at startup and listens for its change, and does not use the History API with path-like addresses, so no catch-all rule belongs to the deployment.
number: F5
binding: must
role: load-bearing
applies_to: [builder]
---

# F5 · The state sits in the fragment, so the server need not be bent

**Applies to:** builder.

**What holds.** The state from F4 sits **after the `#`** (`…/#item/7`). The application reads it at startup and listens for its change; it does **not** use the History API with path-like addresses.

**Why.** The difference is what the server gets to see:

| | path-like address (`/item/7`) | fragment (`#item/7`) |
|---|---|---|
| What the server is asked for | exactly that path | the entry point only |
| What it needs for that | a catch-all rule mapping **every** path onto the same document | nothing |
| What gets deployed | artifact **and** server configuration | artifact |
| Switching while running | change via History API without reloading | change **without any request**, same context |

The core is the third row. **With path-like addresses, a server configuration belongs to the artifact, and it must match the application's views** — it is set up anew on every target system, it goes stale silently when someone adds a view, and it is written differently on different servers. Deployment is then no longer a copy operation, and the core of this concept is gone.

The fourth row is the practical bonus: if only the fragment changes, **the browser loads nothing at all**. Same document, same JavaScript context, same state in memory — the application merely re-renders. Browser back and forward keep working either way.

**Obvious and rejected.** *History API, because the addresses are nicer* — the only reason, and a real one; it costs the freedom from configuration. Whoever has an environment where a catch-all rule is maintained anyway may choose it. *Offer both* — then both routes must be tested, and the application carries two address schemes for the same thing. *No routing at all, everything on one screen* — carries until the second view, after which nothing can be linked.

**How you notice it is violated.** After deploying to a new target system, every address except the start page answers `404`.
