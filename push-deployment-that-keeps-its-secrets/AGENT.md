---
type: Proposal Agent Guide
name: push-deployment-that-keeps-its-secrets
description: Self-contained guide for an agent that sets up the push delivery of a system of several components according to this concept; the steps with the duties of adoption, the rules, an acceptance checklist and common mistakes.
---

# Agent guide

## Purpose

You are setting up the automated delivery of a system of several components, pushed from the deliverer's machine to the host systems, including its access control; or you are reviewing an existing delivery for credentials in the repository, secrets in the package, configurable locks and an order that is only documented. This guide is complete on its own. Rules are named in parentheses as provenance.

## Procedure

**0 · Check the conditions under which the concept no longer holds.** Before building, check with the user: Is there a secrets manager that already does creation, storage and rotation? Does the delivery run unattended from a pipeline? Are the host systems immutable, created anew with every delivery? Does a component have many instances of the same kind? If one of these applies, tell the user what it means and do not silently build on: with a secrets manager the script no longer needs to own the secrets and the instance directories shrink to addresses (F2); unattended, nobody uses a window or reads a `--force` warning, so the configuration rule, the window and the order need a different answer (F5 to F7), and the alternative for that case is `pull-deployment-from-a-registry`, named in the sources of this bundle; with immutable hosts there is no existing configuration to protect (F4, F5); with many instances of the same kind, hand-maintained directories stop holding (F1).

**1 · Name the components and instances.** Which parts are delivered independently, onto which host systems? One script per component, versioned, knowing no target; one directory per instance, outside version control, as the only parameter. The ignore pattern covers all instance directories, not individual names. *Result:* the list of components and instances. (F1)

**2 · Sort the secrets.** For each secret, decide whether it is an access to a host system (exists beforehand, belongs to the operator, is in the instance directory) or a system secret (comes into being during delivery, belongs to the system, created by the script). For each access one component needs for another: which delivery opens the window, which component receives. **Duty of adoption:** write down the list of secrets (per secret: kind, creator, place on the host system, second backup) and read the order of the first delivery off it. If two components would have to give each other accesses, stop and put the cycle to the user; this concept has no answer for it. *Result:* the list of secrets and the order. (F2, F6, F7)

**3 · Build the delivery scripts.** Package contents from the list of versioned files; protective files in otherwise ignored directories versioned explicitly. Runtime data locked in the core of the script, never in the instance configuration. The production configuration is created when missing and never overwritten; the only exception is a `--force` switch that is never the default and names, before it acts, which values it replaces and what that destroys. System secrets are created by the script, written to the host system, backed up beside the instance configuration with narrow permissions and named in the terminal on the first run: the name and the place of the copy, never the value. The window is a small file beside the configuration with a secret and an expiry time, checked by comparing with the clock; the plain text of an access created in it appears exactly once, on the receiving side, never in the terminal; revoking an access works at any time without a window. After the transfer, check from outside what is visible outside, and return a distinct exit code on a finding. Beside each script is its versioned sample configuration. **Duty of adoption:** fix the length of the window and record it with its reason; the duration of the longest run is a reasonable starting point, but that is not proven. Record the wording of the `--force` warning for each system secret. *Result:* one script per component and transfer path. (F2, F3, F4, F5, F6, F9)

**4 · Build the entry point.** Three modes: run targets in the enforced order, create a target (directory with narrow permissions, sample configuration copied in, nothing transferred), list what exists. The entry point copies the template and does not know its contents. *Result:* one command that delivers in the right order without anybody having to remember it. (F7, F8)

**5 · Write the guides for manual operation.** One per component and transfer path, first installation and migration of the store included. They are part of the delivery, not an extra. *Result:* an answer to "how do I do this without the script?". (F10)

**6 · First run, second run.** Run the first delivery per instance in the order from step 2; confirm that the second run changes no file created by the first and that the order no longer matters. *Result:* a delivered system and a repeatable delivery. (F5, F7)

## Rules

MUST: script versioned, instance directory outside, the directory as the only parameter (F1) · system secrets created, placed and backed up by the script, named in the terminal by name and place, never by value (F2) · package from the list of versioned files, protective files versioned (F3) · runtime data locked in the core (F4) · production configuration created, never overwritten; `--force` never default and names what it destroys (F5) · accesses for other components only in the time-limited window, plain text once and never in the terminal; revocation at any time (F6) · order enforced by the tool (F7) · one entry point that runs, creates, lists (F8) · check from outside with an exit code of its own (F9) · a guide for manual operation per component and path (F10).

NEVER: an address, user name or path of a single target in the repository (F1) · a system secret made up or entered by hand (F2) · the value of a secret shown in the terminal (F2, F6) · transferring the working directory (F3) · an exclusion list in the instance configuration (F4) · merging the configuration field by field (F5) · creating machine accesses from the running application; binding revocation to the window (F6) · "deliver A first, then B" only in a guide (F7).

## Acceptance checklist

- [ ] No target name, address or user name in the repository; the ignore pattern covers all instance directories. (F1)
- [ ] Every system secret is created by the script, placed on the target, backed up beside the instance configuration with narrow permissions, and named in the terminal on the first run by name and place, never by value. (F2)
- [ ] Package contents come from the list of versioned files; protective files are versioned. (F3)
- [ ] Runtime data is locked in the core of the script; nothing is exempted in the instance configuration. (F4)
- [ ] A second run changes no file of the first; `--force` exists, is not the default and names its consequences. (F5)
- [ ] Machine accesses arise only in the window; their plain text appears exactly once, on the receiving side, never in the terminal; the window closes by itself after a fixed period; revocation works without a window. (F6)
- [ ] The order of the first delivery is enforced by the entry point. (F7, F8)
- [ ] The entry point can run, create and list; the template is with the script. (F8)
- [ ] A check from outside runs after every transfer, with an exit code of its own on a finding. (F9)
- [ ] Each component and transfer path has a guide for manual operation. (F10)
- [ ] The list of secrets and the length of the window are recorded.

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| One script for all targets, targets as parameters | One script per component; otherwise every peculiarity becomes an `if` (F1) |
| Configuration in the repository, secrets as environment variables | What is created has to stay somewhere: the instance directory (F1) |
| Letting the application create its secrets on first start | The script creates them; otherwise they arise with the first random visitor (F2) |
| Transferring the folder and maintaining exceptions | Transfer the list of versioned files (F3) |
| Deleting everything before the transfer "so that it is clean" | Do not; removed files stay, which is cheaper than the alternative (F4) |
| `--force` that only asks "overwrite?" | Name the values it replaces and what that means (F5) |
| A setup file to be deleted afterwards | A window with an expiry time that closes by itself (F6) |
| The order written into the guide | Enforce it in the entry point (F7) |
| Asking for values interactively when creating a target | A commented template beside the script (F8) |
| A self-check inside the application | Check from outside, in the delivery script (F9) |
| "The script is documented after all" | A guide for manual operation per path (F10) |

## Self-check

Before you report completion: can you name the components and their instances, the list of secrets with the kind of each, the order of the first delivery and why, the length of the window, what `--force` destroys for each system secret, and where the guides for manual operation are? If any of these is missing, you are not done.
