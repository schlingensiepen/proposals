---
type: Proposal Rule
description: The delivery script is versioned and knows no target; the instance directory lies outside version control and knows exactly one; the directory is the only parameter, and the two never share a repository.
number: F1
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F1 · One script per component, one directory per instance

**Applies to:** builder, operator.

**What holds.** The delivery script is **versioned** and knows no target. The instance directory lies **outside version control** and knows exactly one. The only invocation parameter is the directory. The two never belong in the same repository.

**Why.** The separation runs along the question of who may change something: the script is changed by whoever develops the component; the directory by whoever operates the instance. If the two are mixed, every operator has to touch the repository to enter their address, and then it is in there. The cut "per component" instead of "one script for everything" follows the same logic: web space is delivered differently from a machine one runs oneself, and a shared script full of case distinctions becomes unreadable by the third component.

**Obvious and rejected.** *One script with targets as parameters*: saves files and costs sharpness of separation; every peculiarity of a component turns into an `if`. *Configuration in the repository, secrets as environment variables*: works for accesses, not for what is created; a signing key that comes into being on the first run has to **stay** somewhere. *One directory for all instances*: then the credentials of foreign machines lie side by side, and one slip hits all of them.

**How you notice it is violated.** The repository contains an address, a user name or a path that applies to a single target only.
