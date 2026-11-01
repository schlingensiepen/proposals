---
type: Proposal Evidence
description: Where the concept stands; an invented example of a three-component system walked through from the first run to a compromised machine, a second example of the same pattern, and the honest state without a reference implementation.
sources: []
---

# Evidence

## Origin

Reference implementation: none, because none exists yet (status: draft). The procedure is fully decided and specified; the scripts have not been built. The claims of this proposal are therefore **thought through, not proven**; in particular the claim of F7 that the order arises by itself is so far an argument.

## Example: a private inventory system

> **Example** (a private inventory system; names and values invented). Three components: the application on rented web space, an analysis process on a machine with a graphics card, a backup process on a small server.
>
> **What lies in the repository:** `deploy/deploy-sftp.py`, `deploy/worker-scp.py`, a sample configuration beside each, and `deploy/deploy.py` as the entry point. No target name, no address.
>
> **What lies beside it**, ignored by version control: `deploy-webspace/` with address, credentials and, after the first run, the copy of the signing key; `worker-gpu/` and `worker-backup/`, each with address, SSH key and the statement of which task this machine has.
>
> **The first run.** `deploy.py deploy-webspace worker-gpu worker-backup` delivers the web space **first**, not because the guide says so, but because the two others only get their keys while its window is open (F7). In the process the signing key and the deploy key come into being, are written to the server and backed up beside `deploy-webspace/`; the script names them in the terminal (F2). Afterwards it checks from outside whether the data directory is readable and whether a script would be executed in the attachment directory (F9).
>
> Then the two worker targets each fetch **a key of their own** in the open window, one for *analyzing*, the other for *backing up*, and write it into their instance configuration (F6).
>
> **The second run**, a week later, only transfers files: the configuration on the server stays untouched (F5), the store as well (F4), and the order does not matter, because the keys have long been lying locally (F7).
>
> **The unpleasant case.** The machine with the analysis process is compromised. Its key is revoked **immediately**, without a delivery and without a window (F6). The backup key stays untouched, and because it carries a different task, the entire store was never reachable from that machine.

## A second example of the same pattern

The procedure is not bound to a particular access model. If the application instead sets up an administrator with user name and password, nothing in F1 to F10 changes: the password would then be a system secret under F2, created by the script, never made up, named once in the terminal and backed up beside the instance configuration. Only the contents of the window would be different.
