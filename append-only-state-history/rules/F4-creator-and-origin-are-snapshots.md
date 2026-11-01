---
type: Proposal Rule
description: Who created a state, and from where, stands in the state as text, not as a reference to a user record.
number: F4
binding: must
role: independent
applies_to: [builder]
---

# F4 · Creator and origin are snapshots, not references

**What holds.** Who created a state, and from where, stands in the state as **text**, not as a reference to a user record.

**Why.** Only so does the history remain readable when a user is deleted or a device is removed. A reference would either prevent the deletion or punch holes into the history; both are worse than a name that stays.

**Obvious and rejected.** *A foreign key to the user*: prevents deletion or creates dead references. *Recording no origin at all*: then the history is complete and useless.

**How you notice it is violated.** A deleted user leaves gaps in the history.
