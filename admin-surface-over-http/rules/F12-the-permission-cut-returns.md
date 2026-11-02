---
type: Proposal Rule
description: An administration surface in the same process lets the service process write the store; the cut is re-drawn as two processes with two users, only the administration process writing the store, or the waiver stands as a sentence in the service's security considerations.
number: F12
binding: must
role: independent
applies_to: [builder, operator]
---

# F12 · The permission cut returns: two processes, two users, or the waiver is in the security considerations

**Applies to:** builder · operator.

**What holds.** The base cut draws its permission cut between the modes: the service mode reads the store, it is written only in command mode, enforced through users and file permissions. An administration surface in the same process lifts that: the service process **writes the store**, because the administration surface must, and the user-facing surface runs with the same rights. If you build the surface, you therefore draw the cut anew: user-facing surface and administration surface run in **two processes with two users**, and only the administration process writes the store; storing the generated administration secret at first start then also happens there, not in the user-facing process. As long as that is not built, the service's security considerations state (not in a footnote, but as a sentence) that a takeover of the user-facing surface can write the store and thereby issue accesses, and that this is accepted. A waiver that is written nowhere is none.

**Why.** The permission cut is the hardening against the expensive case: a compromised network surface inherits the process's rights, and if that process cannot write the store, the attacker can misuse the function but not repurpose it and issue no accesses. The user-facing surface is the larger attack surface (uploads and external input are processed there), and in a one-process build it is exactly that surface which is beside the one that issues accesses. Separation enforced in the code does not help: anyone who controls the process controls its checks. Splitting into two processes changes nothing at the interfaces (the operations exist once, the pass-throughs call them, only the process in which a pass-through runs is a different one) and is thus the next stage of construction the author names himself and has not yet paid for. Until then the sentence in the security considerations is the minimum: it turns an oversight into a decision a review recognizes as such.

**Considered and rejected.** *Separation in the code ("the user-facing process never calls the write function")*: a declaration of intent an attacker does not read. *Leaving the store writable and trusting that the user-facing surface is tight*: the assumption against which the permission cut was built. *Two containers from the same image, both with the same volume and the same user*: two processes, but no cut. *Not mentioning the waiver because it is "known"*: it is known to the one who decided it; someone adopting the service later finds a service whose network surface can issue accesses, and no document that admits it.

**How you notice it is violated.** The process that handles external input may write the store, and the service's security considerations do not say so; or they say so, and nobody can name what the next stage would be.
