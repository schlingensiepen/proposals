---
name: service-as-single-artifact
type: Proposal Agent Guide
description: Self-contained guide for an agent that builds a small service for one function according to this concept; the eleven steps with the duties of adoption, the rules, an acceptance checklist and common mistakes.
---

# Agent guide

## Purpose

You are building a small service that puts one function on the network, its shape, command set, configuration, packaging and deployment, or you are reviewing such a service for a creeping second tool, second image, management surface or permission model. This guide is complete on its own, except for four things: step 4 needs the choices of the two imported concepts, step 6 needs the choice file of this bundle, the file mechanics of the store (atomic replacement, version marker, lock file) come from a third import, `maildir-style-file-queues`, and what steps 6 and 9 need about the revision the artifact was built from comes from a fourth, `build-stamp`; ask the user for those bundles if you do not have them. Rules are named in parentheses as provenance.

Scope: this is a procedure for building, not an interface design pattern. How the finished service describes itself to its consumers is a separate matter.

## Procedure

Steps 0 to 3 are half an hour of thinking and decide everything that follows. Do not skip them; you will only redo them later under worse conditions.

**0 · Check whether it must be a service at all.** If the caller sits on the same machine, a script, a scheduler, an agent on that host, the answer is a command-line tool, not a service: operating-system permissions are the access control, and network surface, access artifacts and proxy disappear entirely. Only when the caller comes over the network, or will later, does the rest pay off. *Result:* a reasoned decision.

**1 · Write the sentence.** One sentence saying what the service does, with no "and also". If it will not come out, the cut is not found yet. *Result:* the sentence; it becomes the short description and the first line of the command help. (F1)

**2 · Derive the methods.** The sentence yields the main method; add the helper methods without which it does not work: preparation, status enquiry, cleaning up. Whatever will not subordinate itself does not belong in this service. *Result:* the method list. (F1)

**3 · Determine the variant.** What differs from one use case to the next: target system, storage location, limits? The answer is what the configuration entry carries, what the access embodies, and what the caller therefore never chooses. *Result:* the field list of the variant. (F9)

**4 · Define the configuration.** One schema with the global settings; secrets are referenced, never contained. Then **decide the choices of the imported concepts with the user, never on your own**: precedence and the empty value (`one-configuration-one-judgment`), no secret set, unusable, further sources (`where-a-secret-comes-from`). Record the decisions. *Result:* configuration schema and the recorded decisions. (F5)

**5 · Name the state locations.** Configuration and state at separate, named places outside the artifact; the store, the variants and their accesses, lies with the state, created by the artifact itself. **Duty of adoption:** write down, per mode, the user, the directories, and whether reading or writing. *Result:* the directory layout and the permission table. (F6, F8, F12)

**6 · Set up the command tree, before the business logic.** At least: start service · check configuration · show state · set up, change, remove a variant · version, including the revision the artifact was built from (`build-stamp`). Setting up a variant issues its access in the same command. Fixed exit codes, machine-readable output, no interactive input without an argument alternative. **Decide choice W1 with the user:** commands started locally only, or additionally over another channel; with (b), record how the permission cut is established in the channel. *Result:* the runnable skeleton. (F2, F3, F9)

**7 · Build the service mode.** The network surface offers exactly the methods from step 2; the access check looks up in the store what the presented access embodies; there is no administration path and at most one readiness point that says nothing but "running". If the service mode finds a store of an older structure at start, it migrates it before it listens and records the structure version. *Result:* the service, runnable against a hand-written configuration. (F1, F7, F13)

**8 · Finish the setup command.** It configures a variant and creates the access in the same act: show the access artifact exactly once, keep only its hash and a harmless short identifier, rotation as a command of its own. Secrets of the variant (the password of the target system) are taken at invocation over the ranks of `where-a-secret-comes-from`, never as a plain-text argument, and persisted in the store. *Result:* the path from "service runs" to "somebody may use it". (F9, F12)

**9 · Package and set up the delivery.** One deliverable whose entry point is the service mode and against which the commands run in the same environment. **Duty of adoption:** record the form of the artifact (binary, package or directory) and, with several components, how far "one artifact" reaches and who carries administration across component boundaries. **Ask the user explicitly instead of choosing:** container image or package, build pipeline yes or no, deployment file pointing at the published image. With a build pipeline, it starts the built deliverable before publishing and requires that the version command reports the revision built (`build-stamp`). *Result:* the artifact and a delivery directory. (F2, F4)

**10 · Establish the operating form.** Local endpoint; a ready-made reverse proxy in front with the public name and the certificate, or whatever the deployment decides to place there; separate users and file permissions so that the service process reads where the commands write. *Result:* the service is reachable, and the permission cut stands in the process form. (F8, F11)

## Rules

MUST: one service, one function (F1) · one artifact, several modes, no separate tool (F2) · fixed, documented exit codes, machine-readable output, no mandatory interactive input (F3) · one deliverable, named by the deployment file, saying which revision it was built from (F4) · all modes read with the same code, the check is a mode; precedence and secrets per the imported concepts (F5) · state outside, configuration and state separate (F6, F12) · the permission cut in the process form (F8) · configuration and access in one act, secrets of the variant in the store (F9) · local endpoint, TLS with the proxy (F11) · data migration at start (F13).

NEVER: a management surface in the running service (F7) · write access of the service mode to the configuration (F8) · administrative rights or an admin token (F10) · the target chosen by the caller (F9) · TLS in the service, listening on all interfaces (F11) · variants in the configuration file (F12).

## Acceptance checklist

- [ ] One sentence without "and also"; all methods serve it and share one access. (F1)
- [ ] Exactly one artifact with the service and all commands, exactly one deliverable; the form of the artifact is recorded. (F2, F4)
- [ ] The deployment file names the deliverable; local building only as a named exception. (F4)
- [ ] The version command reports the revision the deliverable was built from; the build pipeline publishes nothing that cannot. (F4)
- [ ] Every command has a documented exit code; everything that lists is machine-readable. (F3)
- [ ] Check and start share the code; the decisions on the choices of the imported concepts are recorded. (F5)
- [ ] A version change carries no state along and leaves none behind; the store lies with the state. (F6, F12)
- [ ] No path of the network surface has to be blocked in the proxy. (F7)
- [ ] The service process writes operating data, not the configuration, enforced by users and permissions; permissions per mode are written down. (F8)
- [ ] Setting up issues the access in the same command, shown once, kept only as a hash; secrets of the variant are in the store. (F9)
- [ ] No access can do more than the one function. (F10)
- [ ] The service listens locally; the proxy holds name and certificate. (F11)
- [ ] A new version migrates old data at start and records the structure version. (F13)
- [ ] The decision on W1 is recorded; with (b), the permission cut in the channel.

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| A separate little admin tool beside the service | Make it a mode of the same artifact; two tools mean two configuration parsers with two version numbers (F2) |
| A management web UI "for convenience" | Commands on the server; whoever still needs it takes the extension via W1 (b) and pays for it (F7) |
| Deployment file builds locally | Point it at the published artifact; otherwise there are as many artifacts as machines (F4) |
| Second, "slim" admin image | One image; a second one can carry a different version and see something else than the service (F4) |
| Service process runs as the user that owns the configuration | Separate users and directories: the service reads configuration and writes operating data only (F8) |
| Caller passes the target as a request parameter | The access embodies the target; a chosen parameter is authorization code that can be wrong (F9) |
| An admin token "for emergencies" | Do not create one; administration is configuration, and it happens on the server (F10) |
| "We will add access tokens later, it is only me for now" | Build them in now: created alongside the configuration it costs nothing; retrofitting hits when the second caller arrives (F9) |
| A second function added because the service is running anyway | Second service; otherwise callers must be told apart, and roles and a permission model follow (F1) |
| TLS terminated in the service | Reverse proxy in front; otherwise certificate renewals become service restarts (F11) |
| Variants kept in the configuration file | Store in the state directory, created by the artifact itself (F12) |
| A migration command somebody forgets | Migrate at start (F13) |

## Self-check

Before you report completion: can you name the one function in one sentence, the one artifact, the one deliverable, the user that writes configuration and the user that runs the service, and the recorded decisions on every choice? If any of these is missing, you are not done.
