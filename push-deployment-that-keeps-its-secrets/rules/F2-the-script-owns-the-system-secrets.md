---
type: Proposal Rule
description: Whatever must come into being during delivery, signing keys, deploy keys, machine accesses, the script creates, writes to its place on the host system and backs up beside the instance configuration; in the terminal it names the secret and the place of the copy, never the value; no human makes one up, none is ever in version control.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · The script owns the secrets of the system

**Applies to:** builder.

**What holds.** Whatever must come into being during delivery, signing keys, deploy keys, machine accesses, **the script creates**, writes to its place on the host system and additionally backs up beside the instance configuration. In the terminal the script names the secret and the place of the copy, never the value. No human makes one up; none is ever in version control.

**Why.** A secret made up by hand is the weaker one, and one entered by hand travels through clipboards, chat windows and sticky notes. Above all: anyone who leaves the creation to a human has no point in time at which it happens **safely**; it happens whenever somebody thinks of it.

**The second backup is not a luxury.** Some system secrets are irreplaceable: a signing key with which permanent addresses were signed makes every one of those addresses worthless when it is lost, and a data backup usually does not include it, because it backs up data, not configuration. That is why a copy is kept beside the instance configuration, with narrow permissions, and the script names it in the terminal on the first run, by name and place, not by value.

**Considered and rejected.** *Letting the application create the secrets on its first start*: then they come into being with the first random visitor, and nobody knows when. *Having them entered into the instance configuration*: the path by which they end up in clipboards. *Doing without the second backup*: holds exactly until the host system is lost.

**How you notice it is violated.** Somebody asks which value they should enter where.
