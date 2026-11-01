---
type: Proposal Rule
description: All application logic hangs on exactly one address that points at a directory and is resolved to a script by the server's default document; no path behind it, no technology marker, no rewriting.
number: F1
binding: must
role: load-bearing
applies_to: [builder]
---

# F1 · One address, reached through the default document

**Applies to:** builder.

**What holds.** All application logic hangs on exactly one address that points
at a **directory** (`/api/`) and is resolved to a script by the server's default
document. No path behind the directory, no technology marker, no rewriting.

**Why.** Without rewriting an address is either a file or a directory — there is
no third option, and both major web servers answer `404` to anything else. The
default document is the only resolution rule every server masters in its core;
it is thus the smallest common assumption one can make. That the address
incidentally no longer names a technology is a bonus, not a reason: an
`index.php` in the address turns any later change of language into a breaking
change for every caller.

**Obvious and rejected.** *Paths behind the script file*
(`/api/index.php/items/7`) — works on some configurations and not on others; it
depends on a setting you can neither see nor change on someone else's hosting.
*Application logic in the query string* (`/api/?p=items.search`) — the address
becomes unambiguous, but every parameter lands in the access log, and nobody
keeps the line between "control" and "secret" for years. *Assume rewriting and
document it* — pushes the problem onto the operator and fails on platforms that
do not know the module at all.

**How you notice it is violated.** The API runs on one hosting package and answers `404`
on another, with no change to the code.
