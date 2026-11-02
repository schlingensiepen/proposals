---
type: Proposal Scope
description: Security considerations, what the separation and its review cost, where it applies, when this no longer holds, what it does not solve, and what is not yet proven or settled.
---

# Scope

## Security considerations

| Risk | Typical course of events | Countermeasures |
|---|---|---|
| Internals move into the product | During the handover a workshop document is copied along that nobody checked | The handover is a deliberate act (F3) with a review offered before every handover that asks whether names, access paths or statements about third parties are taken over (F7); workshop documents are not passed on as they are; their statement and reason are taken over |
| The existence of the workshop is revealed | A reference in the product points at a non-public repository | No references from the product to the workshop (F2); a dead reference also reveals the name |
| Secrets in the workshop history | Credentials end up in a draft and stay in the history | The separation does not protect against this; it only limits the readership. Secrets do not belong even in a non-public repository; if the workshop is ever published, the history is the problem |
| Accidental publication of the wrong thing | The embedded directory is picked up by the outer repository | Ignore rule in the two-roots form (F5); in the submodule form the entry type itself takes care of it |
| Unnoticed change in the product | A contributor writes without agreement | Announcement rule (F6) |
| Wrong assignment of states | Without a recorded reference, the wrong state of the product is assumed for a working state | Solved by the reference in the submodule form (F4); in both forms every release names its state and purpose, as a decision in the workshop and a release tag in the product (F8), which in the two-roots form is the only record |

Limit of the model: the separation protects against accidents, not against intent, and it does not turn a non-public repository into a secure one. Anyone who has access to the workshop sees everything in it; the separation only decides who needs access.

## What it costs

**Two repositories are two repositories (F1).** Two permission administrations, two clone operations, two histories to search, two places where something might be. For a single person working alone that is little; for a team it is onboarding effort.

**The handover is manual work (F3).** Carrying over statement and reason instead of linking takes time, and somebody has to decide what goes across. That is intended, but it is work at every publication.

**Redundancy between workshop and product (F3).** What was taken over exists twice. It is not maintained by hand but generated anew in full from the source when needed; between two generations the product is knowingly out of date as a whole, which is bearable as long as nothing in it claims the opposite.

**Submodules are a learning hurdle (F4).** If you do not know them, you clone the work repository and find an empty directory. That is a known stumbling block and costs every newcomer one moment of confusion.

**Without a submodule the assignment is missing (F5).** The frugal form costs exactly what F4 brings: you no longer know which state of the product belonged to a working state, unless somebody writes it down; F8 writes it down at every release.

**The review costs rounds (F7).** Two reviewers of other models than the builder, and up to three rounds, take time at every handover; a standing approval saves the question, not the review.

**The workshop grows without limit (F1).** Because its history is a diary, it is never tidied up. That is as it should be, and it still means the workshop becomes harder to survey over time.

## Where it applies

### Preconditions

**There really are two kinds of content.** Where everything that arises is published anyway, the separation is effort without return.

**The permission management allows separate visibility.** The whole construction depends on one of the two repositories being able to be held back.

**For the submodule form: tools that can write submodule references.** If they are missing, F5 applies, and that is not a shortcoming but the intended second path. The tool situation decides between the two forms, not preference.

### When this no longer holds

**The genesis itself is to be published.** Some projects show their genesis deliberately; then there are no longer two zones of trust, and the separation becomes mere order, which subdirectories achieve as well.

**Several products from one workshop.** With three or four embedded product repositories the construction becomes cumbersome, and the question of a tool for managing several repositories arises anew.

**Rules demand traceability beyond publication.** Where it must be proven from which requirement a published line comes, the copy rule of F3 is not enough; then a continuous linkage is needed, and that contradicts F2.

**The workshop becomes the product.** When drafts are themselves what others are to receive, as with a series of dossiers, the repository concerned is a product repository, and this concept does not apply to it.

### What it does not solve

**Confidentiality.** A non-public repository is neither encrypted nor tamper-proof; the separation governs visibility, not protection.

**Traceability from requirement to code.** As above: the copy rule preserves reasons but no continuous linkage.

**The question of what belongs in the publication.** The rules govern *how* the handover happens, not *what* belongs across. The concept gives a starting point, the usual allocation of workshop and product, and makes deciding and recording the allocation a duty of adoption; what belongs across in a given project is still decided there, from its purpose.

**History maintenance in the product.** How changes are bundled and named there is a different topic.

### Not yet proven

* Version 0.12 is a draft: complete in content, not yet reviewed by the author.
* Long-term operating experience is missing. Whether the redundancy from F3 can be maintained over time is no longer the question: it is not maintained but generated anew from the source (F3).

### Not yet settled

* Whether the direction should be reversible, the product repository embedding the workshop, when the workshop is public as well. F2 forbids it for good reasons in the normal case; the special case "both public" has not been thought through.
* How to handle several products from one workshop: from what number a tool for managing several repositories pays off, and whether that changes the rules or only the handling.
* How the pattern relates to publication pipelines that build and publish from the workshop. Then it is no longer only knowledge that moves but a built artifact, and where its build pipeline lives is open.
