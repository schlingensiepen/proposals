---
type: Proposal Rule
description: The envelope is JSON-RPC 2.0, unchanged; no homegrown format and no extra fields at envelope level.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · An established envelope, not a homegrown one

**Applies to:** builder.

**What holds.** The envelope is JSON-RPC 2.0, unchanged: `jsonrpc`, `method`,
`params` as an object, `id`; in the response `result` **or** `error`. No
homegrown format, no extra fields at envelope level.

**Why.** If you have one endpoint, you need a field saying what is meant — that is
unavoidable once the address says nothing. Inventing that wrapper yourself costs
the same work as adopting a finished one and adds the single disadvantage that
nobody knows it. JSON-RPC has been unchanged since 2010, fits on two pages, and
requires no library: the envelope can be read and written in twenty lines. On
hosting without a package manager, that is the decisive criterion.

**Considered and rejected.** *A homegrown, "simpler" format* — same cost, without
anyone else knowing it; and the seemingly superfluous fields of the standard get
reinvented one by one at the first error case. *GraphQL* — pays off with many
heterogeneous clients making flexible queries over deep graphs; with a flat
model and few known access patterns you pay for a library and a query language
for nothing, and file uploads are a special case there. *REST with resource
addresses* — the most obvious choice, and it fails on F1: resource addresses
cannot be expressed without rewriting.

**How you notice it is violated.** The envelope has gained a field the standard does not
provide for — and a second client ignores it.
