---
type: Proposal Rule
description: Rank 1 is an environment variable whose name the service proposes; the name is in the usage description and in the start report.
number: F2
binding: must
role: load-bearing
applies_to: [builder, operator, consumer]
---

# F2 · Environment first, under a canonical name

**What holds.** Rank 1 is an environment variable whose name the
service proposes (pattern `<SERVICE>_<PURPOSE>`, say
`IMAPSYNC_ADMIN_TOKEN`). The name appears in the usage description and
in the start report.

**Why.** The canonical name solves the problem of finding the secret
again across sessions and machines: the service itself says where its
secret usually lives. And the environment is the way a platform hands
its secret in without disk and without log; whoever has that needs no
other rank.

**Obvious and rejected.** *The operator picks the name*: then the usage
description cannot say where the secret lives, and every instance is
named differently. *A generic name (`TOKEN`, `PASSWORD`)*: collides with
the second service on the same machine.

**How you notice it is violated.** Two operators of the same software
name the variable differently; or somebody has to read the code to find
out what it is called.
