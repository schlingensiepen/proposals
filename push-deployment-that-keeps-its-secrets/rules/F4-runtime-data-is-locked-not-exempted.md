---
type: Proposal Rule
description: What arises on the host system at runtime, the store, attachments, backups, and the production configuration once it has been created, the script never touches; the lock is in the core of the script, not in a configuration file.
number: F4
binding: must
role: independent
applies_to: [builder]
---

# F4 · Runtime data is locked firmly, not exempted by configuration

**Applies to:** builder.

**What holds.** What arises on the host system at runtime, the store, attachments, backups, and the production configuration once it has been created (F5), the script **never** touches. This lock is in the **core** of the script, not in a configuration file.

**Why.** A lock that is configurable is a lock that somebody configures. And the mistake it prevents cannot be repaired: an overwritten store is gone. The instance configuration may decide on **addresses and accesses**, not on what may be deleted.

**Considered and rejected.** *An exclusion list in the instance configuration*: convenient, and the most likely road to data loss; it gets copied, shortened, adapted. *Deleting everything before the transfer "so that it is clean"*: that is a synchronization, not a deployment; removed files stay where they are, and that is cheaper than the alternative.

**How you notice it is violated.** The instance configuration contains a path that is to be exempted from a deletion.
