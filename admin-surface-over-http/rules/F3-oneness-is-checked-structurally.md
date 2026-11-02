---
type: Proposal Rule
description: That neither command line nor administration surface performs an operation itself is held by the structural test of the imported one-operation-many-surfaces; what this extension adds is that it runs over both pass-throughs with the same list of operation names.
number: F3
binding: must
role: load-bearing
applies_to: [builder]
---

# F3 · Oneness is checked structurally, not by behavioral tests

**Applies to:** builder.

**What holds.** That neither the command line nor the administration surface performs an operation itself is held by the structural test in the sense of `one-operation-many-surfaces`, and that concept applies unchanged: a whitelist of permitted imports, the names of the operations in no pass-through, no file system access except named exceptions, every command and every handler calls at least one operation, each assertion seen red once with its protection removed. **What this extension adds:** the test runs over **both** pass-throughs, the command line included, with the same list of operation names on both sides; and it is one of the conditions under which one may have the second surface at all.

**Why.** The imported concept justifies why a structural property needs a structural check. What is said here is only why it holds for both surfaces of this extension: without the structural test the one operation of F2 is an intention that falls at the first wish for a helper function; if F3 falls, nobody knows whether F2 still holds. And in the author's implementation the command line was the older copy; it had accumulated just as much logic of its own as the new surface.

**Considered and rejected.** *The test for the new surface only*: the command line was the older copy; it had accumulated just as much logic of its own.

**How you notice it is violated.** A pass-through of the command line or of the administration surface gets a helper function of its own, "just for the presentation", that reads the store, and no test turns red.
