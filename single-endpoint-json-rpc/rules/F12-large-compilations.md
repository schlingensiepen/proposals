---
type: Proposal Rule
description: Backups and exports are commissioned, polled, fetched and terminated through methods on the endpoint; the path exists only after completion, termination is mandatory for security-relevant data, and critical data may get a lazily enforced expiry.
number: F12
binding: must
role: load-bearing
applies_to: [builder, consumer]
---

# F12 · Large compilations: commission, poll, fetch, terminate

**Applies to:** builder, consumer.

**What holds.** When the application has to **compile** larger datasets — a
backup, an export — this does not happen through a fixed path but through
methods on the endpoint:

| Method | Job |
|---|---|
| `…commission` | commissions the compilation. Answers with the download path when produced immediately — otherwise with a **job handle** |
| `…status` | only for concurrent compilation. Reports the state and, **only once finished**, the download path, formed in the usual file pattern (F11) |
| `…terminate` | ends the provision: file and path disappear. **For security-relevant data this method is mandatory**, not optional |

**Additionally, an expiry may be defined for critical data**, after which a
provision lapses even without termination.

**Why.** A provision takes the same route as an attachment (F8, F11) and
therefore looks like one — but the damage is different: an attachment's address
exposes **one attachment**, a backup's address **the entire dataset**. The
justification from F8 — "the damage stays bounded" — therefore does not hold
here, and without a rule of its own the backup would silently inherit a
justification that never applied to it. The envelope cannot carry the bytes, but
it can carry the **commission**; so the control — request, poll, terminate —
stays in the API, and only delivery sits outside. The three paragraphs below
justify the three parts individually.

**Why the address comes into being only after completion.** If it existed
earlier, half a file would sit under a valid address — and a caller cannot tell
an incomplete backup from a complete one. A state is a statement, a path is a
promise; while compilation runs there may only be the statement. This holds
regardless of whether work is concurrent — it is merely unavoidable there.

**Why termination is a method of its own and does not happen automatically.**
The server does not know when a retrieval is finished; an aborted download looks
to it like a successful one. The **retriever** knows. So it reports it, and the
same call clears up. The lifetime thus hangs on an event that is actually
observed, rather than on a clock nobody winds.

**Why the expiry belongs there anyway — and how it is enforceable without a
scheduled process.** If termination does not happen, because a retrieval aborts
or a process dies, a complete copy of the dataset lies around. For critical data
that is unacceptable. But a cheap hosting package has no reliable timer. The
resolution is **lazy enforcement**: every call to one of the three methods
clears expired provisions along the way. It costs nothing and needs nothing — it
has the gap that nothing is cleared while nobody commissions anything (not yet settled; see
the scope).

**Obvious and rejected.** *Treat the provision like an attachment, i.e. store it
under a permanent path* — convenient, and the actual danger; a backup sitting
permanently under an address is a data leak waiting for that address to end up
somewhere. *Stream the compilation through the envelope* — produces exactly the
memory demand F8 avoids. *Only an expiry, without a terminating method* — then
the file lies around until it lapses although it was fetched long ago; the
expiry is the fallback, not the normal case. *Only termination, without an
expiry, even for critical data* — relies on the confirmation always arriving.
*Return the path at commissioning time and let the caller poll whether the file
is there yet* — moves the distinction "not yet" versus "broken" onto the client,
which cannot make it.

**How you notice it is violated.** The provisioning directory contains files from last
week — or a retrieval yields a file that will not open.
