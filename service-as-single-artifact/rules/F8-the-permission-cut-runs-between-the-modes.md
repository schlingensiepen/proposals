---
type: Proposal Rule
description: Configuration is written only by commands and read by the service mode; operating data including the store is written by both; enforced through users and file permissions in the process form.
number: F8
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F8 · The permission cut runs between the modes

**Applies to:** builder · operator.

**What holds.** What lies outside the artifact falls into two kinds, and the permission cut follows this division. **Configuration** — what the service is to do and for whom — is written exclusively by the commands; the service mode only reads it. **Operating data** — queues, run state, scratch areas, everything that arises in performing the function — is written by the service mode itself; the commands may write there as well, for cleaning up and intervening is their job. The service mode otherwise runs with the narrowest permissions sufficient to run. Both are enforced through separate users and file permissions in the process form, not through intent in the code: the configuration location and the operating-data location are different directories with different permissions. The store, the variants and accesses the setup command creates, belongs to the operating data: commands create it, the service mode reads it and writes it only when migrating at start (F13); see F12. That commands are started locally is the precondition of this cut; if they enter the system over another channel (W1 b), the cut has to be re-established there. Which permissions exactly are needed per mode depends on the service and is to be fixed on adoption; the agent guide makes this a duty.

**Why.** A compromised network surface inherits exactly the permissions of the service process. If that process cannot write the **configuration**, neither can the attacker — they can misuse the function but not repurpose it, cannot issue accesses and cannot shift a target; the takeover becomes limited damage. For the **operating data** this does not hold, and it would be pointless as well: the service has to write there in order to do its work. The separation is therefore not a hardening against everything but one against repurposing — and that is precisely the expensive case. A separation enforced in the code does not help with it: whoever controls the process also controls its checks.

**Obvious and rejected.** *Both modes as the system administrator* — convenient, and the cut is worthless. *One service user with full access to everything* — the same convenience, only with a better conscience; with it the separation between configuration and operating data collapses. *Configuration and operating data in the same directory* — then the permissions can no longer be distinguished without administering every file individually. *Separation in the code only ("the service never calls the write function")* — a declaration of intent that an attacker does not read.

**How you notice it is violated.** The service process is allowed to write the configuration file — or configuration and operating data lie so close together that nobody can say who may change what.
