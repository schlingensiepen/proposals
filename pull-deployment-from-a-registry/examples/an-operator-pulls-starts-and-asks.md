---
type: Proposal Example
description: A walk-through of an operator who is not the builder through the chain of this concept, with invented names and values, including the cached image after a fix under the same moving name.
---

# Example: an operator pulls, starts and asks

What follows is the path of an operator who is not the builder through the chain of this concept — with invented names and values, as illustration; it proves nothing the evidence does not already prove, and at two points it shows what the reference implementation does not have.

The operator receives a delivery directory and chooses the deployment file for the server in the trusted network. The header tells them, in this order: that the file is without TLS and that administration secret, accesses and the mailbox password travel in clear text — so not onto the open network; that it names `registry.example.org/team/mailsvc:1.4`, a moving name that follows version 1.4, and that whoever wants to pin a state enters `sha-<revision>` from the build log instead (F3, F6); that no login is needed, because the anonymous fetch of the manifest was checked (F5); and the four steps: pull, start, read the secret from the log, and then compare the version information of the running instance with the build log (F7). They carry them out. The pull fetches the image; the start reports "healthy" after half a minute, because the same health check that the smoke test waited for now runs at the operator's (F2). The version information reports `build: 4f1c…`, and the build log of version 1.4 names the same state. Only now does it count as rolled out on their side.

Three weeks later version 1.4 appears with a fix — the same moving name, a new image behind it. The operator restarts the container and, out of habit, asks for the version information: `build: 4f1c…`, unchanged. The restart took the image from the local cache; the name was the same, so the runtime did not pull (F7). They pull explicitly, start again, ask again: `build: 9a07…`, as in the build log of the fix. Without the question they would have taken the fix for installed — exactly the incident from the evidence, except that here it stood in the deployment file and therefore came as no surprise.

What occurs in this walk-through and is missing from the reference implementation are two sentences in the header: the one about the kind of name and the one about the check call. What is missing from both is the check of the outer edge (F8): the operator has no proxy in front, so there is no advertisement the network would have to honor — as soon as they put one in front, the part this concept treats as open begins for them.
