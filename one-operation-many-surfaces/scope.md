---
type: Proposal Scope
description: Security considerations, what the concept costs, the precondition under which it applies, and what is not yet settled; when this no longer holds and what it does not solve are not yet written.
---

# Scope

## Security considerations

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Two writers on the store | the command and the second surface write at the same time; a change is lost | version stamp on read, expected on write, non-blocking lock in a file of its own (as `service-as-single-artifact` requires for its store): properties of the one operation, not of a surface (F1); visible over HTTP as `ETag` and `If-Match` |

Further security considerations: not yet written.

## What it costs

**Every operation has two openings that need maintaining (F1).** The operation exists once, but a pass-through has one opening per operation, and both want updating when an operation gets a field. The structural test prevents a second implementation arising there; it does not prevent an opening being forgotten. The author's implementation had two commands without an HTTP counterpart and noticed it by a claim that was not true; and to this day it has three store-wide actions that exist over HTTP only per account, so the operator without a shell has to assemble them from the account list. An opening that spans everything on one side and only one item on the other escapes every test and reaches the operator only at the twentieth account.

**The structural test is code, and it ages (F2).** A whitelist of permitted imports, a list of operation names, a named exception: every new function, every new operation, every renaming touches it. And with every change it wants the red check: remove the protection, see red, restore. If you skip that, you have a list of names in which the one name is missing.

## Where it applies

### Preconditions

**A service whose operations exist as callable functions.** The pass-throughs presuppose that there is something to pass through to (F1): actions that know neither HTTP nor process exit. A service that keeps its logic in its commands must extract it first, before it may build a second surface. You recognize the situation by a command that does more than read arguments, call a function and print.

### When this no longer holds

Not yet written.

### What it does not solve

Not yet written.

### Not yet settled

* **How far the structural test of F2 goes.** The rules demand that every command and every handler calls exactly one operation. The author's implementation checks imports, names, file system access and that every command calls an operation and everything it calls is one; it does not check that every handler calls **exactly one** operation and does nothing between call and return. Whether the sharper form is maintainable, or the whitelist is already the limit of what can be said about structure and the rest belongs in the behavioral test over both surfaces, is open.
