---
type: Proposal Scope
description: Security considerations, what self-containment costs, where it applies, when the answer flips, what it does not solve, and what is not yet settled or proven in this working version.
---

# Scope

## Security considerations

| Risk | Typical course of events | Countermeasures |
|---|---|---|
| Confidential material travels along with inlining | When reasons are taken over, internal names, customer data or access paths are copied along | When inlining, take over the *matter*, not the context; before a repository is published, review all knowledge brought in for confidential material |
| Outside knowledge taken over without the right to do so | A section of somebody else's documentation is copied in | Inlining means statement and reasons in your own words, not taking over somebody else's wording; evidence belongs in the sources |
| An instruction becomes an attack surface | A file that an agent follows unchanged is altered by somebody who may write to the repository | Treat changes to instruction files like code changes, with the same review and the same history; whoever may write instructs an agent to act |
| False security from an appearance of completeness | A document looks complete although its prerequisites for understanding are dead | This is exactly what F1 is directed against; the check question belongs in the review, not only in the intention |

Limit of the model: self-containment says nothing about correctness. A repository can be fully understandable on its own and still document wrong decisions; this concept only ensures that the decisions, with their reasons, are visible and thereby open to challenge.

## What it costs

**Writing effort when authoring (F5).** Writing out statement, reasons and rejected alternatives takes considerably longer than setting a link, and the benefit only arrives when somebody else reads the document or the decision is questioned. That is the least popular kind of work: expensive now, valuable later.

**Double maintenance (F6).** Every change of content touches several places. Whoever does not organize that gets contradictions; whoever organizes it pays with every change.

**Longer documents (F1, F5).** Self-containment produces repetition, and repetition produces length. Whoever regards brevity as a value in itself will find these documents unpleasant.

**The structure wants maintaining (F2, F3).** Tasks change; an entry point that branches to three instructions, two of which are outdated, is worse than none.

**Generation needs a chain (F7).** Whoever checks in generated files needs a run that makes deviations visible; otherwise the note on the source is a mere claim.

**The yield is invisible.** A self-contained repository strikes nobody; all that is missing are the questions, the mistakes and the resurrection of rejected ideas. That makes it hard to defend when somebody sees the costs and not the benefit.

## Where it applies

### Preconditions

**The repository is the basis of the work.** Where the work actually happens in a wiki or a ticket system and the repository contains only code, the concept has nothing to hold on to: you cannot bring knowledge to a place where nobody reads.

**There are recurring activities.** A repository without contributions, a pure archive, needs neither entry point nor task definitions.

**Somebody maintains it.** Self-containment without maintenance (F6) lasts about as long as the first change.

### When the answer flips

**Regulatory uniqueness is prescribed.** Where a quality system requires that a statement exists exactly once and is controlled there, inlining is ruled out; then only the link remains, and self-containment ends at that place. It is more honest then to name it explicitly as not achievable instead of claiming it.

**The knowledge belongs to somebody else.** Outside documentation cannot be brought in, only summarized and cited. Where the summary is not enough, a genuine prerequisite remains.

**The repository is too large for one entry point.** With very many sub-areas, the entry point becomes either incomplete or a directory listing; then self-containment is needed per sub-area, and the question of what holds them together arises anew.

**The content changes faster than redundancy can be maintained.** Then contradictions are more likely than dead links, and the calculation reverses.

### What it does not solve

**Correctness.** See the limit of the model under the security considerations: fully understandable does not mean correct.

**Findability across repositories.** Whoever does not know that this repository exists is not helped by its entry point.

**Permissions and approval.** Who may write to the repository is a different question; F1 only makes it more important, because instruction files have effect.

**The form of the documents.** This concept says what must be contained, not in which format or structure.

### Not yet settled

* Whether the check rule of F1 can be supported by machine. A tool finds dead links; the actual question, whether a living link is a prerequisite, probably not. Would a convention help that marks references as "evidence" or "prerequisite", so that the latter stand out at all?
* Where the limit of inlining lies for outside norms and standards. Their statement can be summarized, their wording not taken over. Whether that needs a rule of its own, for instance: bring in the *effect*, cite the *location*.
* How self-containment is kept up in a repository that grows very large over time (see when the answer flips): whether it then holds per sub-area, and what holds the sub-areas together.
* How this concept relates to the widespread conventions for agent entry files: whether it complements or replaces them. At present one file is treated as leading and the other points to it (F2); whether that remains the right answer depends on how the tools develop.
* Whether a self-contained repository needs a statement on keeping the decision log. Under F5 it is no longer a prerequisite for understanding, but it is the history, and losing history is a different loss from losing understandability.

### Not yet proven

* This is a working version: complete in content, not yet reviewed by the author.
* Operating experience over a longer period is missing. The series in which the concept is applied was a few weeks old when this version was written; in particular it is open how well F6 can be kept up in practice. The evidence from the test with a context-free agent is strong, but it is a snapshot and not operation over months.
