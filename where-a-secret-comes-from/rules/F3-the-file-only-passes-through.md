---
type: Proposal Rule
description: Rank 2 is a file with tight permissions used only as a pass-through; its content flows straight into the call and never appears in output, logs, error messages or the context window.
number: F3
binding: must
role: load-bearing
applies_to: [builder, operator, consumer]
---

# F3 · The file only passes through

**What holds.** Rank 2 is a file readable by the service's user only
(owner-only read; on a looser mode the service refuses to start and
says so), used only as a pass-through: the content flows straight into
the call and never appears in output, logs, error messages or the
context window. The path is named through the environment under the
canonical name with the suffix `_FILE` (`IMAPSYNC_ADMIN_TOKEN_FILE`) or
has a documented default inside the service's state directory; a secret
the service generates itself ([W1 (b)](../choices/W1-no-secret-set.md))
is stored at exactly that path.

**Why.** The file is there for the operator who cannot set a variable,
and for the service that must store a secret it generated. Cut narrowly
as a pass-through it remains a place, not a second channel to the
outside.

**Obvious and rejected.** *The secret in the configuration file*: that
one gets versioned, copied, pasted into tickets. *Client-side encryption
acrobatics*: the process cannot keep secrets from itself; the
protection is only apparent.
*Copying the file into a variable at start and passing it on*: then it
sits in the environment of every child process.

**How you notice it is violated.** The file's content shows up in a log
or an error message; or the file sits in the same directory as the
versioned configuration.
