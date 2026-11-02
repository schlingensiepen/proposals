---
type: Proposal Rule
description: That no pass-through performs an operation itself is held by a structural test in the build that parses their source; whitelist of imports, banned operation names, no file system access, every command and handler calls exactly one operation, each assertion seen red once.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · Oneness is checked structurally, not by behavioral tests

**Applies to:** builder.

**What holds.** That no pass-through performs an operation itself is held by a **structural test** in the build, a test that parses the source of the pass-through modules and fails where an operation is performed there instead of called. It checks at least four things: that a pass-through knows from its own service **only** the operations and the operational parameters (a **whitelist** of permitted imports, not a blacklist; a blacklist lets through every module nobody thought of); that the **names of the operations themselves** (for instance: read and write the store, generate and verify a token, touch queue and pool, connect to the target system) appear in no pass-through; that no pass-through touches the **file system**, neither opens nor writes nor lists, and where an exception is needed, it is in the test **by name**, not as a pattern; and that **every command and every handler** calls exactly one operation; a command that calls none does something itself. Each of these assertions is run once while writing it with its protection removed, and must turn red.

**Why.** This is the load-bearing idea of this proposal, and it is more general than its occasion: **where a property is structural, the check must be structural.** A behavioral test calls the surface and checks what comes out, and two correct copies of the same operation pass every behavioral test until one of them is changed. The ten places from F1 were all green. A test that reads the source, by contrast, sees the copy itself: a command that writes the store imports the write function; a handler that generates a token names the generator; a pass-through that searches instead of asking opens a directory. The whitelist instead of the blacklist, because the next copy comes in through a module nobody thought of; the named exception instead of the pattern, because a narrower pattern silently admits the next reader. In the author's implementation exactly one function of a pass-through reads a file, the password file the operator names on invocation, and the test names that function. And the red check belongs to it, because a test about structure stays green particularly easily without checking anything: a list of names in which the one name is missing is an assertion that looks like protection.

**Considered and rejected.** *Behavioral tests that do the same task over both surfaces and compare the results*: useful as an addition, but they find only what the task touches, and they find the copy only once it is already drifting. *Code review*: does not recognize a useful helper function in a pass-through as a violation, because it is useful. *A blacklist of forbidden imports*: checks the modules one thought of. *The test for the new surface only*: the command line was the older copy; it had accumulated just as much logic of its own.

**How you notice it is violated.** A pass-through gets a helper function of its own, "just for the presentation", that reads the store, and no test turns red. Or the quiet form: the test is green, and when the protection is removed it stays green.
