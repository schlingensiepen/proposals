---
type: Proposal Choice
description: Whether a platform's secret manager is a rank of its own in the ranking or a feeder to the environment and the file.
number: W3
granularity: per-setting
applies_to: [builder, operator]
---

# W3 · Further sources

**The choice.** Whether a platform's secret manager (or an operating
system agent, a hardware module) is a rank of its own in the ranking or
a feeder to ranks 1 and 2. Decided per secret, in practice per group.

**(a) A rank of its own** between the pass-through file and the
once-only channel. Consequence: the service addresses the manager itself
and knows it as a source;
[F4](../rules/F4-the-higher-rank-wins-and-never-hides.md) and
[F5](../rules/F5-replacing-is-two-moves.md) apply to it too (report on
duplicate setting, way to clear). Fits where the service already
links a library of the manager and where the manager cannot write into
the environment or a file.

**(b) A clause.** Permitted as long as it is at least as safe as the
environment and the file; no rank of its own, no report on duplicate
setting, because the service does not know the source. The manager hands
the secret into the environment or into the file and is thereby a feeder
to rank 1 or 2. Fits where the platform can do that (most can) and keeps
the service free of manager code.

**Rejected for all options.** *A secret manager as a prerequisite*:
then the service runs only where there is one.

**Record your decision.** Per group; if in doubt, (b).
