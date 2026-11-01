---
type: Proposal Rule
description: Tokens are generated server-side with at least 128 bits of entropy and a recognizable prefix; the service keeps only the hash and a short ID, and the plaintext token appears exactly once, with no retrieval function.
number: F2
binding: must
role: load-bearing
applies_to: [operator]
---

# F2 · Creation: server-side, high-entropy, visible only once

**Applies to:** Operator.

**What holds.** Tokens are generated exclusively server-side (CSPRNG, at least 128 bits of entropy) and carry a recognizable prefix (`svc_…`, say), so that secret scanners find leaks and nobody confuses tokens. Of the token, the service stores **only the hash** (for high-entropy random tokens, fast SHA-256 suffices; password KDFs address guessable secrets) plus a harmless short ID for logs and administration. The plaintext token appears exactly **once**, at creation; a retrieval function does not exist — loss means rotation, not research.

**Why.** One-time display minimizes the number of copies to exactly those the recipient creates; hash storage ensures that a leaked config file or backup on the operator's side is not a keyring; and the prefix turns every accidental commit into a hit that tools find, instead of a secret that lies unnoticed for years.

**Obvious and rejected.** *Self-chosen tokens with password character* — people again choose what they can remember, and what they can remember is what they already use elsewhere. *A "show token" function for the loss case* — every re-display surface is a query surface; the experience from operations: its absence produces inquiries at first ("I have lost it") — and exactly the rotation discipline one wants. *Tokens by mail or ticket "for convenience"* — turns the mailbox and the ticket system into a keyring.

**How you notice it is violated.** A token can still be "looked up" somewhere weeks later — at the operator's, in a mailbox, in a ticket.
