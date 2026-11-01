# Agent entry point

This repository is a library of technical and organizational proposals ("bundles") that lets the user of an AI agent bring pre-packaged best practice into their project: each is readable as a discussion paper and usable, unchanged, as a work instruction for AI agents.

You are here for one of two jobs. Read only the file for yours:

| Your job | Read |
|---|---|
| **Create or maintain** a proposal: new bundle, revision, status change, moving a dossier of the previous form into bundle form | [agents/create-or-maintain.md](agents/create-or-maintain.md) |
| **Find and use** knowledge: locate the right proposal, decide what to load, apply it | [agents/find-and-use.md](agents/find-and-use.md) |

Iron rules for both jobs:

- Every bundle stays understandable and followable with every link outside it dead (self-containment).
- Everything is English; the German vocabulary lives in each bundle's `terms.md`.
- Rule and choice numbers (F1…, W1…) are stable; a retired number stays retired.
- Nothing is reinterpreted retroactively; versions and status change only through the bundle's `log.md`.
- A bundle never states another bundle's status, version or content; it names other bundles by slug. The root README's register is the only place for status.
- Attribution in the text is "the author", never personal names; the `author` field in a bundle's `README.md` head is the one place that carries the name.
- Choices are decided by the user, not by the agent: an agent adopting a proposal presents each choice with its options and consequences and records the user's decision.
