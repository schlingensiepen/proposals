---
type: Proposal Rule
description: Nothing in the description needs inside knowledge to resolve, and every identifier the caller must substitute comes with the call that yields it, in the same sentence.
number: F18
binding: must
role: load-bearing
applies_to: [builder]
---

# F18 · Closure: no reference the reader cannot resolve; every identifier is handed out first

**Applies to:** builder.

**What holds.** Everything the delivered description mentions or demands MUST be resolvable by the reader with what he has: the description itself, the answers of the service and the documents the entry page links. Two rules follow. **Outward:** no requirement identifiers, decision numbers, names of files or modules of the repository, no project jargon; nothing that only resolves with a document the caller is not given. **Inward:** for every placeholder the text asks the caller to substitute (an identifier in an address, a value in a field), the description names the call whose answer yields that value, in the same breath as the introducing word ("answers with …"), not somewhere nearby. Both belong in a check in the build that runs over every description at every nesting depth, including the description fields of the interface description.

**Why.** A description is a chain: a link that demands something no other link supplies forces the reader to guess, and guessing is exactly what self-description is meant to spare him. And what only resolves with inside knowledge is inside knowledge dressed up as information: for whoever writes the description, identifiers are the traceability he himself needs; for whoever has only the URL, they are noise, as two independent reviewers put it. The rule for the placeholder is drawn so narrowly because its first version was worthless: the name came up once more in the neighboring sentence, and the check was satisfied.

**Obvious and rejected.** *Leaving identifiers in because they help the builder look things up*: they help him in the source code; in what is delivered they help nobody who is meant to read them. *Delivering a glossary of the internal identifiers along with it*: turns inside knowledge into outside knowledge and makes the entry page fat; whoever does not need the identifier to act does not need the glossary either. *Checking the chain only for the "important" identifiers*: the observed case concerned precisely the identifier of the call that takes effect; every other identifier of the surface had been introduced.

**How you notice it is violated.** A reviewer without inside knowledge reports the description as noise for a caller who has only the URL, or a reader asks whether a call produces a value at all that it has always produced. And the quiet form: a check catches *one part* of the inside knowledge (decision numbers, file paths) and lets the other parts through all the less noticed, because it creates the impression the topic is done; the builder considers the description clean, and the next outside reviewer finds identifiers nobody ever looked for.
