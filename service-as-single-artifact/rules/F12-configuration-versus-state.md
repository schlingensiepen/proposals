---
type: Proposal Rule
description: The boundary between configuration and state runs at the point in time; the variants and their accesses are state, the store, created by the artifact itself and written with atomic replacement, a version marker and a lock of its own.
number: F12
binding: must
role: load-bearing
applies_to: [builder]
imports: [maildir-style-file-queues]
---

# F12 · Configuration versus state: what arises in operation is state

**Applies to:** builder.

**What holds.** The boundary between configuration and state runs at the point in time, not at the format: configuration is set before the start and read at the start (F5); everything that arises or changes in operation is state, even when it looks like a configuration sheet. The variants of the function and their accesses, which the setup command creates (F9), are therefore state: the **store**. It is in the state directory (F6), belongs to the operating data (F8) and is created by the artifact itself, in command mode, with tight permissions, instead of being supplied from outside. Because a command creates a variant while the service runs and reads the same store, this is a concurrent system: every piece of state that more than one process can write gets the mechanics for it: it is **replaced atomically** (write a new file, `fsync`, rename over the old one; the file mechanics of `maildir-style-file-queues`), it carries a **version marker** that a writer reads first and expects unchanged when it writes, and a **lock of its own** in a separate file, because the rename exchanges the inode of the store file itself. The marker and the lock are this concept's requirements; only the atomic replacement is imported.

**Why.** The way of thinking called "configuration" brings a file that comes from outside: with an owner other than the service, with a start abort because of permissions, and without any protection against two simultaneous writers, because in that way of thinking a human writes configuration before the start. Exactly that was the error of the first implementation, which put the variants into the configuration file: they are written by a command at runtime, and the service reads them in the same moment. The separation that carries is not "file versus database", a file is enough, but configuration versus state.

**Considered and rejected.** *The variants as a configuration file one "just edits"*: the starting point; it cost an owner other than the service, a start abort and two unprotected writers. *A database "because concurrency"*: solves the writing problem and brings a second process, a schema, a migration and a backup; out of proportion for a store of a few dozen entries.

**How you notice it is violated.** The service aborts at start because of the permissions of a file that does not even belong to it; or two commands run shortly after each other leave a store in which the change of the first is missing.
