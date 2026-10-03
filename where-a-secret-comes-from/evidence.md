---
type: Proposal Evidence
description: Where the concept has held; its origin in four places of the series, the latent access incident in a container deployment, and the named exception for the generated first secret.
---

# Evidence

## Origin

Merged from four places in the series that each stated the same ranking
for their own role: the service that reads (`service-as-single-artifact`);
the agent that obtains (`capability-tokens-for-agents`); the usage
description that prescribes (`self-describing-services`); the
administration surface that generates (a contribution from September
2026). An earlier working title, "the first secret with no way in", was
wrong: the environment is a way in, before the start; the first secret is
not a special case but the lowest rank of the same ranking.

## The latent access

Occurred in a container deployment by the author: when the
administration secret was changed, the environment variable was changed;
the secret generated at first start stayed at rank 2, known to nobody,
and would have applied again the next time the variable was missing. The
fix was the start report
([F4](./rules/F4-the-higher-rank-wins-and-never-hides.md)) and the
change in two moves ([F5](./rules/F5-replacing-is-two-moves.md)). The
repository concerned is not publicly accessible.

## The named exception

The same deployment supplied the case for
[W1 (b)](./choices/W1-no-secret-set.md): when an image was pulled,
nobody but the service was inside, so only it could generate the first
secret, and the log was its only return channel. The build has since
verified that the secret appears in the log exactly once and not again
after a restart.
