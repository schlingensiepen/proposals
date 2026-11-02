---
type: Proposal Rule
description: File names are <nnnn>-<short-title>.md, lowercase ASCII; never renamed after acceptance, and a number is never reused.
number: F4
binding: must
role: independent
applies_to: [writer]
---

# F4 · The file name carries number and descriptive title

**Applies to:** writer.

**What holds.** File names have the form `<nnnn>-<short-title>.md`: four-digit
running number, hyphen, short title in lowercase with hyphens, ASCII only. The
name is not changed after acceptance, and **a number is never reused** — not
even one freed by a discarded draft. Gaps in the sequence are left open.

**Why.** The number sorts and is citable; the title makes the directory listing
readable without opening anything — the whole body becomes its own table of
contents. Four digits, because three-digit numbers break sorting at a thousand
records and the fix would then touch every pointer. **No reuse**, because
numbers get cited — in meetings, in source comments, in other documents. A
reused number makes every earlier citation silently wrong: the pointer still
resolves to an existing record, just the wrong one. A dead pointer is harmless;
a redirected one is dangerous. Lowercase and ASCII so that
file systems and encodings never become the topic — a body travels between
machines, and a non-ASCII character in a file name is the difference between
"works" and "works most of the time".

**Considered and rejected.** *Number only (`0031.md`)* — shorter, but the directory
listing becomes a column of digits telling nothing. *Title only, without a
number* — sorting becomes alphabetical and thus meaningless, and citations grow
long. *Date instead of number* — two decisions on one day collide, and renaming
to resolve it is forbidden (see above).

**How you notice it is violated.** The directory listing sorts alphabetically instead of
chronologically — or a pointer to a file name is dead because someone sharpened
the title.
