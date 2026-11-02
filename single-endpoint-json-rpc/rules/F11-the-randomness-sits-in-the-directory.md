---
type: Proposal Rule
description: Every file gets its own subdirectory named with a cryptographically random identifier and keeps its upload name inside it.
number: F11
binding: must
role: load-bearing
applies_to: [builder]
---

# F11 · The randomness is in the directory, not in the filename

**Applies to:** builder.

> This rule describes the mechanism F8 and F9 use. It is placed back here because
> numbers are never reassigned; in reading order it belongs between F8 and F9.

**What holds.** For binary data there is a **writable area the web server serves
as an entirely ordinary file area** — no application code, no special handling.
Within it **every file gets its own subdirectory**, named with a
**cryptographically random** identifier (a UUIDv4 or 128 bits from a secure
random source, in hex). The file inside **keeps its name from the upload**. The
address therefore reads:

```
<base>/<area>/<random-guid>/<original-filename>
```

**Why.** The randomness is in the directory and not in the filename — for
three reasons that together force the construction:

- **The randomness has to be somewhere**, or the address is derivable and F8 is
  void. The directory is where there is room for it, because nothing else has to
  go there.
- **The filename stays meaningful.** Anyone who saves the file gets
  `invoice-2026.pdf` and not `9f2c4a1e…`. A random filename would need a
  `Content-Disposition` header to heal that — and only someone with application
  code in the serving path can set one. That is exactly what F8 excludes. **The
  construction is therefore not taste but a consequence of F8.**
- **Name collisions are impossible** without anyone renaming anything: two files
  of the same name are in different directories.

**The identifier must come from a cryptographic random source.** A counter, a
timestamp or a language's usual weak random function makes the address
predictable — and thereby renders F8 worthless without anyone noticing.

**Considered and rejected.** *Random filename in one shared directory* — saves a
directory, costs the meaningful name; besides, a folder with ten thousand files
is noticeably slow on some file systems. *Structure by date or object*
(`/2026/08/entry-7/`) — makes the address derivable; anyone who knows an object
guesses its files. *Content hash as identifier* — tempting, because it merges
duplicates, but the hash is computable: anyone who holds the same content knows the
address. *Both versions of a file (original and thumbnail) in one directory* —
tempting, because the folder would then be "worth something even without the
database". It is not: the folders are named after random values; without the
database nobody knows what they belong to, whether one file is in them or two.

**How you notice it is violated.** From one known file address a second can be guessed —
or a request without a filename shows the directory contents.
