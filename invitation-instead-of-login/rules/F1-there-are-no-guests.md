---
type: Proposal Rule
description: Every access to content requires a credential, reading included; possession of a hard-to-guess address never suffices.
number: F1
binding: must
role: load-bearing
applies_to: [builder]
---

# F1 · There are no guests

**Applies to:** builder.

**What holds.** Every access to content requires a credential, reading included. There is no preliminary stage in which possession of a hard-to-guess address suffices for reading.

**Why.** A reading stage of "whoever has the address may look" seems harmless and is the most expensive part of the system: it makes the unguessability of the address the **sole** protection of everything reachable through it, with no way to revoke, no traceability, no limit on trying. And it grows: what shows "only the overview" today shows the history including every participant's name after the next extension.

The price is lower than it seems. The alternative would be convenient if setting up were laborious, but it consists of **one click**, which anyone makes anyway who wants more than to look at a single page.

**Obvious and rejected.** *Reading free, writing protected*: the classic split, and it assumes reading is harmless; with records carrying history, names and locations it is not. *Unguessable address as access protection*: it protects against trying out identifiers, it does not replace a credential; it cannot be withdrawn and sits in every access log that ever saw it. *"Open for now, secure it later"*: the securing never comes, because by then it would lock out existing users.

**How you notice it is violated.** There is an address at which content is visible without a credential, even if "only" an overview.
