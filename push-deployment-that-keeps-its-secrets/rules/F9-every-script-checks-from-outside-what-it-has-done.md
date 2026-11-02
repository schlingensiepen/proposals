---
type: Proposal Rule
description: After the transfer the script checks the result for the questions that can only be answered from outside, such as a publicly readable directory or a file served that should be executed; the result is shown in the terminal, and a finding produces an exit code of its own.
number: F9
binding: must
role: independent
applies_to: [builder]
---

# F9 · Every script checks from outside what it has done

**Applies to:** builder.

**What holds.** After the transfer the script checks the result, specifically the questions that can **only be answered from outside**: Is a directory publicly readable that must not be? Is a file served that ought to be executed, or the other way round? The result is shown in the terminal, and a finding produces an exit code of its own.

**Why.** A checking program **on** the host system cannot answer "what does a stranger see?": it would have to call itself over the network and might bypass precisely the intermediate server that matters. Only an observer outside sees what is visible outside, and the delivery script is outside.

**And it is the only moment at which anybody looks.** Afterwards the system runs, and an open directory is noticed only when it is too late.

**Considered and rejected.** *Giving the application a self-check*: it can check everything except what matters. *The check as a separate operation that one triggers now and then*: "now and then" is never.

**How you notice it is violated.** A misconfiguration of the host system is noticed in a security incident and not at delivery.
