---
type: Proposal Rule
description: Every token is individually revocable and rotatable without interruption; rotation is a routine command, and expiry dates belong in the operations calendar and monitoring.
number: F8
binding: must
role: load-bearing
applies_to: [operator, user]
---

# F8 · Rotation and revocation hit exactly one purpose

**Applies to:** Operator · User.

**What holds.** Every token is individually revocable and individually rotatable, and both without interruption: the new token is valid in parallel with the old one until the old one is deleted. Rotation is a routine operation (one command, one motion), not a project. Expiry dates — wherever tokens or the secrets of their issuing channels are time-limited — belong in the operations calendar and in monitoring; the adopter settles the rhythm (see the agent guide).

**Why.** The value of the narrow cut is realized only in revocation: when decommissioning a token costs exactly one purpose, a case of suspicion becomes an operational procedure instead of surgery — and then rotation actually takes place. The empirical record of the neighboring proposals of the series is unambiguous: expiring secrets and revoked accesses are the most frequent long-term causes of failure, and they surprise precisely those operations that have no expiry date in the calendar.

**Obvious and rejected.** *"Tokens never expire, that is more convenient"* — convenience beats hygiene exactly as long as it takes for the first leak to be years old and unnoticed. *Big-bang rotation of all tokens on a cutoff date* — couples the failures that the narrow cut has just decoupled.

**How you notice it is violated.** A revocation forces the re-setup of uninvolved purposes — or an expiring secret surprises operations.
