---
type: Proposal Scope
description: What this fragment covers and what not, the security points still to be worked out, what it costs as far as known, where it applies, when this no longer holds, and the open question of object trees.
---

# Scope

> **Fragment, deliberately unfinished.** This bundle is a reminder, not a finished concept. It records what is decided so that it is not lost, and names the one question it hinges on: **how the procedure carries over to object trees.** The rules were developed on a **flat** model (one item, a sequence of states, attachments on them) and tried out there; whether they hold when the thing itself has a tree structure is open and written out below under what is not yet settled. The author has thoughts on this that have not yet been incorporated.
>
> **If you use this fragment, you must know:** for flat items it is applicable. For trees it is **not an answer**, but an open question with some groundwork.

## Security considerations

*Fragment, not yet worked out.* To be treated at least: what an immutable set of records keeps permanently about persons (F4 writes names down, and they can no longer be removed); whether a duty of erasure is compatible with immutability; and how the living metadata entry (F3) is protected against somebody using it to swap content that is already being displayed.

## What it costs

*Fragment.* Known so far: more records; the need to manage attachments separately; the impossibility of correcting a mistake silently: a wrong entry stays visible and is corrected by a new state, not overwritten.

## Where it applies

### Preconditions

An item with a manageable number of changes, and attachments that do not change with every change.

### When this no longer holds

As soon as the item itself has a **structure**, a tree of parts that are changed individually, it is not decided what "a state" refers to. This fragment leaves that condition open; see the first point under what is not yet settled.

### What it does not solve

Everything that has to do with relations; see the first point under what is not yet settled.

### Not yet settled

* **How does this carry over to object trees?** This is the question that makes this bundle a fragment. For a flat item, "a state" is unambiguous. If the thing has a tree structure, a document with sections, a parts list with assemblies, the question arises what a change refers to:
  * **State of the subtree:** changing a leaf creates a new state only there. The history grows with the **amount of change**, but there is no longer a consistent view of the whole tree: "what did the whole tree look like on March 3?" has to be assembled from partial states.
  * **State of the whole tree:** every change creates a new overall state. The consistent view of the whole tree is trivial, but the history grows with the **size of the tree** instead of with the amount of change, and F2 would have to hold for subtrees, not only for attachments.

  Both ways have a price, and the choice probably depends on whether the domain needs a consistent view of the whole tree. The author has thoughts on this that have not yet been incorporated. Until they are, this bundle remains a fragment.
* **Does F3 hold in a tree as well?** If a living metadata entry is tied to the "latest" state: what is the latest state in a tree in which work goes on at several places at the same time?
* **How does immutability go together with a duty of erasure?** F4 writes names down as text so that the history survives deletions. Exactly that makes them impossible to delete. For a private system this is acceptable; as soon as personal data is collected on a larger scale, it is an open question.
