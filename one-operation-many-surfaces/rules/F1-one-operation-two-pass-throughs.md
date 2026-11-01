---
type: Proposal Rule
description: Every administrative action exists exactly once as a callable function that knows neither HTTP nor process exit; the command line and the second surface are pass-throughs onto it, each with one translation function and the same field names.
number: F1
binding: must
role: load-bearing
applies_to: [builder]
---

# F1 · One operation, two pass-throughs: no surface has logic of its own

**Applies to:** builder.

**What holds.** Every administrative action exists **exactly once**, as a callable function that knows neither HTTP nor process exit: it takes arguments, acts, and returns a result or an error kind of its own vocabulary. The command line and the second surface are **pass-throughs** onto it: each handler and each command reads its arguments, calls exactly one operation and returns what came back. What a pass-through may have of its own is the **translation**, exactly one function per surface that maps the operation's error kinds to status codes or exit codes respectively, and the **presentation**: where a line breaks, what goes to which channel, what a human sees instead of a machine. What both surfaces emit carries **the same field names**, because they are the same response models; the machine-readable output of the command is the structure of the second surface. No surface writes the store, touches the file system or talks to the target system; the operations do that, and only they. A pass-through that needs something no operation provides gets a new operation, not a search of its own.

**Why.** The price of the opposite has been measured, not guessed: in the author's implementation the operations first existed twice, and a later comparison found ten places where the two had drifted apart. One reported more on a failure than the other, one knew a server feature the other did not read, one turned every unknown status into a server error, one had one status line fewer. Each foundation had to be hardened twice, two bugs fixed twice each, and one description advertised behavior the other side had long removed. Two implementations of the same operation drift apart as soon as someone touches only one, and nobody notices, because each works on its own. The one translation function per surface is the place where HTTP knowledge and exit-code knowledge may live without any operation having to know it, and it is one place, not thirty, so that an error kind one surface treats differently from the other is a visible decision and not an accident. The same field names, finally, are what "replaces the command completely" means for a script: it can learn on one surface what it reads on the other.

**Obvious and rejected.** *A shared library both surfaces use*: that is the operation, correctly; rejected is the form in which the library holds only helper functions and each surface assembles its own flow from them, because then the flow exists twice. *The second surface first, and the command line calls it over the network*: then the command needs the secret of the second surface, a running service and a port, and the shell loses exactly the advantage it has, authentication by server access. *The command line first, and the second surface calls it as a subprocess*: parsing text output, guessing exit codes, and an administration process that starts processes. *Building one surface "more completely" because one is at it anyway*: then it is no longer a pass-through and the other is incomplete.

**How you notice it is violated.** The same administrative action behaves differently on the two surfaces and nobody decided it; or a bug in handling the target system has to be fixed twice; or a field has a different name on one surface than on the other.
