---
type: Proposal Rule
description: The upload endpoint takes one file and returns one handle, nothing else; the directly served upload area is protected by three barriers graded by obligation.
number: F9
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F9 · File intake carries no application logic

**Applies to:** builder, operator.

**What holds.** The upload endpoint takes **one file** and returns **one
handle**. Nothing else: no reference to an object, no database row. What the
file belongs to is stated afterwards by an ordinary RPC call quoting the handle.

**Why.** The reason is not tidiness but **atomicity**. An operation with three
files happens as: upload three times, then **one** application call with three
handles. If the third upload breaks off, nothing has come into being. The
reverse route — create the object first, then attach files — leaves half an
object behind on every abort; and where history is immutable, that cannot be
cleaned up. The second reason is speed: file intake is the slowest request in
the system, and everything you load onto it lengthens precisely the request most
likely to hit a time limit.

**The area the upload writes into is served directly by the web server** (F8,
F11). That makes it the most dangerous spot in the whole system: an uploaded
script file would be **executed** there on retrieval, and a directory listing
would turn "unguessable" into plain "enumerable". Three barriers, in descending
obligation — and the order matters, because on cheap hosting the lower two can
fail:

1. **Required: check the actual content type**, not the declared one. The
   declared type comes from the caller and is therefore evidence of nothing.
   This check is application code and therefore works **always** — it is the one
   barrier you can rely on.
2. **Required: put an `index.html` and an `index.php` into every served
   directory**, both empty or carrying one sentence. They suppress the directory
   listing **without any server configuration**, because every web server serves
   a default document instead of enumerating a folder — exactly the mechanism
   from F1, used a second time. Two files, because servers differ in which they
   prefer and you do not know which applies. **Without this barrier F8 is
   worthless:** a listing shows every identifier at once, and then nobody has to
   guess.
3. **Recommended: switch off script execution in the target directory.** This is
   the most effective barrier and at the same time the only one that requires a
   **server configuration** — an `.htaccess` the operator may not even have
   evaluated, or a `web.config` on the other platform. On a package whose
   configuration you can neither see nor change, this is a measure you **take
   and do not assume**. Whoever declares it mandatory builds their security on an
   assumption they cannot verify.

**The grading is the point.** Barriers 1 and 2 cost nothing and work everywhere;
barrier 3 works best and can fail silently. That is why none of the three may
stand alone — and why barrier 3 must not be the one you rely on.

**Obvious and rejected.** *Have the upload attach the file right away* — saves
one call and costs atomicity. *Clean up never-attached files on a timer* — such
a rule eventually deletes while someone is still typing; orphaned files are
cheaper than that failure mode, and they are findable by comparing directory
against database.

**How you notice it is violated.** After an aborted upload, half an operation sits in the
data.
