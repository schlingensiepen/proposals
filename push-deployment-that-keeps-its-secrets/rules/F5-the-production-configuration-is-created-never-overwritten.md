---
type: Proposal Rule
description: If the production configuration is missing, the script creates and fills it; if it exists, it stays untouched, field by field too; the only exception is an explicit, never-default switch that names what it destroys before it acts.
number: F5
binding: must
role: independent
applies_to: [builder, operator]
---

# F5 · The production configuration is created, never overwritten, except on instruction

**Applies to:** builder, operator.

**What holds.** If the production configuration is missing on the host system, the script creates and fills it. If it exists, it stays **untouched**, field by field too. **Exception:** an explicit switch (`--force`) that must not be the default and that **names what it destroys** before it executes.

**Why.** On the first run there is nobody else to create the random values, so the script must be able to create them. On every later run the file contains values the script does not know and cannot reconstruct; overwriting it would be a silent total loss.

**Why the switch must exist nonetheless:** without it there are situations one cannot get out of: a changed deploy key, a changed address, a damaged file. The answer "then by hand" is none, because that is exactly where the mistakes happen that the script is meant to avoid. **A dangerous operation belongs in the tool, not outside it**, but explicitly, named, and never as the default.

**What the switch has to say before it acts:** which values it replaces and what that means. For a signing key, for instance: that every address signed so far is invalid afterwards. A `--force` that only asks "overwrite configuration?" shifts the decision onto somebody who does not know the consequence.

**Obvious and rejected.** *Always overwrite and read the values out beforehand*: presupposes that one can read them out; with a damaged system that is exactly what fails. *Never overwrite, by hand if need be*: pushes the most dangerous operation to where there is no check and no warning. *Merging field by field*: sounds gentle and is the worst case: afterwards nobody knows which value came from where.

**How you notice it is violated.** A second run of the script changes a file that came into being on the first, or there is no way to replace a compromised key.
