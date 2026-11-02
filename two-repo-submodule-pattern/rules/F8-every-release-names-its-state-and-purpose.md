---
type: Proposal Rule
description: Every release reads in full "state x of the artifact, released for purpose Z" and is bound to a versioned, frozen state; the release for delivery is recorded in the workshop as a decision and marked in the product repository with a release tag, which in the two-roots form replaces the missing submodule reference.
number: F8
binding: must
role: independent
applies_to: [author]
---

# F8 · Every release names its state and its purpose

**Applies to:** author. In both forms; in the two-roots form it is the only link between working states and product states.

**What holds.** Every release reads in full **"state x of ⟨artifact⟩, released for purpose Z"** and is always bound to a versioned, frozen state. The release for delivery is recorded in the workshop as a decision; it names the state that is handed over, and in the product repository a **release tag**. In the two-roots form this replaces the missing submodule reference.

**Why.** Without a recorded reference it is lost which state of the product belongs to a working state, unless somebody keeps it (F5). The release is the moment at which this is known for certain, so that is where it is kept: the decision in the workshop, where the reason lives, and the tag in the product, where the consumer sees it, without any reference from the product to the workshop (F2). A release that names no purpose says nothing about what it permits; a release of a state that keeps changing approves nothing. The point in time is bound to the actual step of publication, not to a version name.

**Considered and rejected.** *Not recording the assignment at all*: then nobody can say which state of the product belonged to a working state (F5). *A release without a purpose*: a state released for a test is not released for delivery. *Binding the release to a version name instead of a frozen state*: a name can move on while the state it was meant for changes. *Writing a reference to the workshop into the product*: forbidden by F2; the tag carries the state, the decision in the workshop carries the rest.

**How you notice it is violated.** Somebody cannot say which state of the product was released for what; a release decision names no state or no purpose; or a published state of the product carries no release tag.
