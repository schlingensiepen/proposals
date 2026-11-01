---
type: Proposal Sources
description: What this proposal builds on; the external sources, named but not yet checked against the originals.
sources:
  - id: maildir
    title: D. J. Bernstein, the maildir format (qmail)
  - id: posix-rename
    title: POSIX specification of rename()
---

# Sources

## Imports

This concept imports no other proposal.

## Related

None named. The proposal that imports this concept is listed in [`used-by.md`](./used-by.md).

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

These references were taken over from the design work. They have not yet been checked against the originals, and no retrieval date is recorded; read them as pointers, not as verified citations.

* D. J. Bernstein, the maildir format (qmail) [^maildir]: the original pattern, one message per file, `tmp`/`new`/`cur`, lock-free through atomic rename. It stands here for the origin of the procedure and the evidence of its viability (see the [evidence](./evidence.md)).
* POSIX specification of `rename()` [^posix-rename]: the atomicity guarantee within one file system on which [F3](./rules/F3-every-transition-is-an-atomic-rename.md) rests. It stands here for the technical foundation whose limits (file system boundaries, network file systems) determine the preconditions in the [scope](./scope.md).
* The author's draft-filing service, repository `imap-service`: the reference implementation in the evidence. The repository is not publicly accessible.

[^maildir]: D. J. Bernstein, maildir format description for qmail; reference not yet checked.
[^posix-rename]: The Open Group, POSIX `rename()`; reference not yet checked.
