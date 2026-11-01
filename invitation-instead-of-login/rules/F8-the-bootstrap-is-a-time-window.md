---
type: Proposal Rule
description: Setup-only procedures require the strongest secret, held outside the store, and a time window opened by a deployment; afterwards they answer that the window is closed.
number: F8
binding: must
role: load-bearing
applies_to: [builder, operator]
---

# F8 · The bootstrap is a time window, not a delete-me file

**Applies to:** builder, operator.

**What holds.** Procedures that may only exist during setup, creating the first administrator, creating machine keys, migrating the schema, require **two things**: the system's strongest secret, held outside the store, **and** a time window opened by a deployment. Afterwards the same procedures answer that the window is closed.

**Why.** A fresh system has nobody who could permit anything; it must be able to unlock itself without standing open permanently. The time window solves that without inventing a second secret: whoever can deploy has access to the configuration anyway; they cannot obtain rights they did not already have; the bootstrap merely makes them **conveniently reachable for a limited time**.

**Why the secret must lie outside the store:** it is needed precisely when the store is empty, damaged or mid-migration. Were it inside, it would be unreachable at the moment it is needed.

**How the window comes about in practice**, *example, deliberately plain:* the deployment script writes a small file next to the configuration on every run, containing two things: the **setup secret** and an **expiry timestamp** (say, an hour later). The setup procedures check both; once the timestamp has passed, they answer that the window is closed. No timer is needed, no background process, no state in the database, only a comparison against the clock.

**And here lies the actual justification of the whole procedure:** whoever can write that file **has access to the machine anyway**. They could replace the application, read the store, do anything. So the bootstrap gives them no rights they did not already have; it merely makes them **conveniently reachable for a limited time**. That is precisely why it is no contradiction that a window exists in which more is possible than usual: the trust boundary does not run at the window but at write access to the machine, and it was already there.

**Obvious and rejected.** *A setup file you delete after installation*: the classic route, and it relies on somebody remembering; it is forgotten in exactly the cases where it would matter. *A separate setup secret*: one more place where things can drift, with no gain over the one that already exists. *Opening and closing the window by hand*: then it stays open.

**How you notice it is violated.** There is a file whose presence decides whether somebody can take over the system.
