---
type: Proposal Choice
description: Whether commands are started locally by somebody with server access, or brought into the system over another channel that must re-establish authentication and the permission cut.
number: W1
granularity: per-service
applies_to: [builder, operator]
---

# W1 · Channel of the commands

**Applies to:** builder, decides with the user. Per service.

**The choice.** How commands enter the system.

**(a) Started locally** by somebody who has server access. Consequence: that access is the authentication (F7), the permission cut runs through users and directories (F8); the service needs no login of its own for administration. Fits where somebody with a shell is at the target.

**(b) Brought in over another channel**: an HTTP surface, a watched directory, a queue. Consequence: the channel needs an authentication of its own and introduces a secret that can do more than the function; the permission cut (F8) has to be re-established in the channel, two processes, two users, or the omission is stated in the security considerations. Fits only where nobody with a shell is at the target. The HTTP form of (b) is the extension `admin-surface-over-http`.

**Rejected for all options.** *An administration path behind a simple login in the main surface*: the weakest login of the system guards its strongest effect (F7).

**Record your decision.** Per service, with the reason; with (b) additionally how the permission cut is established in the channel.
