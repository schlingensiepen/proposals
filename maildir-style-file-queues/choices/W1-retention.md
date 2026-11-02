---
type: Proposal Choice
description: Whether done and failed jobs (the journal) may disappear without an explicit deletion, by age or under a size quota, or whether nothing is deleted automatically; uploads in the pool are excluded in every option.
number: W1
granularity: per-service
applies_to: [builder, operator]
---

# W1 · Retention of the journal and the disk

**The choice.** Whether done and failed jobs (the journal) may disappear
without an explicit deletion, and whether the service enforces a size
quota. Decided per service by the builder together with the operator.
**Uploads in the attachment pool are excluded in every option**: they are
never deleted automatically; that stays a rule
([F8](../rules/F8-the-service-deletes-no-payload-data.md)). In every
option, the staging folder is cleared of unfinished leftovers older than
one hour, and nothing else is cleared without a decision recorded here.
If no decision is made, (a) holds.

**(a) Nothing is deleted automatically.** Everything except staging
leftovers is deleted by the caller or by an operator command.
Consequence: the journal stays complete; the disk fills up at some
point, visibly through the command that shows the counts
([F9](../rules/F9-visibility-not-automation-and-only-inward.md));
cleaning up is a recurring operator task. Fits where an operator looks
at the service regularly and attachments are reused by many jobs (a
logo, a form, a signature).

**(b) A retention period for the journal, by age.** Done jobs older than
N days are cleared; failed jobs only after a decision by the operator.
Consequence: the journal does not grow without limit; after the period,
it is missing when something goes wrong. Fits where the journal is
needed only briefly, for troubleshooting.

**(c) A quota, by size.** When the quota is exceeded, the service evicts
the oldest done jobs, never uploads, never waiting or failed jobs, and
reports it in the log. Consequence: the disk does not fill up unnoticed;
the quota is an operating value somebody has to set. Fits where nobody
looks after the service regularly, or where the disk is small and hard
to extend.

**(d) (b) and (c) together.** A period for the journal and a quota for
the disk; the consequences of both apply.

**Rejected for all options.** *Delete after use*: misjudges the reuse of
attachments. *Expiry times for uploads*: arbitrary, and they hit the artifacts in permanent use.

**Record your decision.** Per service, with the reason; for (b) the
period; for (c) the quota and its unit (per access or per service); for
(d) both. If in doubt, (a).
