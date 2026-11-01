---
type: Proposal Agent Guide
name: pull-deployment-from-a-registry
description: Self-contained guide for an agent that writes or reviews the build pipeline, the image names, the shipped deployment files or the rollout steps of a service delivered as an image from a registry; procedure for builder and operator, rules, acceptance checklist, common mistakes and self-check.
---

# Agent guide

## Purpose

Apply this guide when a service is built as a container image, published to an image registry by a build pipeline, and started by an operator who pulls it from there — nobody pushes anything onto the target machine — and you are writing or reviewing the build pipeline, the naming of the images, the deployment files that ship with the image, or the steps an operator follows to roll out and verify a new build. This guide is complete on its own; rule numbers in parentheses are provenance only.

Two roles use this guide; apply the rule set for yours.

- **Builder** — you own the source of a service that ships as one container image. A build pipeline (any build service) builds the image and pushes it to an image registry (any registry — the one of your source host, your own, a third party's). Operators you may never meet pull the image with a deployment file you ship (a Compose file or equivalent). You never write onto their machines.
- **Operator** — you run such a service. You have the image name, the shipped deployment file, a container runtime, and a way to ask the running instance which build it is. Nobody deploys for you; you pull, start, and check.

Preconditions this guide assumes and does not establish: the image carries the source revision it was built from (a build stamp; the imported concept `build-stamp` governs it) and the running service can report it; all state and all secrets live outside the image; the build environment can start containers; the target machine can reach the registry. If a precondition does not hold, say so to the user before you start.

The concrete forms below — tag names such as `sha-<revision>`, the header of a deployment file, the list of smoke-test assertions — are patterns, not a house convention: the words are interchangeable, what they must achieve is not.

## Procedure

### Builder

1. **Confirm the model.** Nobody has or wants write access to the operator's machine. If a push channel exists and is meant to be used, this is a push deployment, and the alternative `push-deployment-that-keeps-its-secrets` applies instead. (F1)
2. **Make sure the image carries its source revision and the running service reports it;** without that, the last assertion of step 4 and the operator's verification are impossible. (F2, F7 precondition)
3. **Write the tag computation:** sanitized branch name, version tag, immutable `sha-<revision>`, and the default name enabled for the default branch only. (F3, F4)
4. **Write the smoke test:** build into the local runtime, start with a throwaway state location and throwaway values, wait for the image's own health check, hit every published interface, verify the service's own guarantees, restart and verify the secret again, then remove the container, create a new one from the same state location and verify once more, and compare the reported revision with the built one. Make it the step before publishing, and run it once with an assertion removed to see that it turns red. (F2)
5. **Publish in a separate step after the smoke test.** Decide whether the published image is carried over by digest from the tested build or rebuilt from cache, and say which in the pipeline. (F2)
6. **Add the visibility measurement:** an anonymous manifest fetch as a step without the build credential, or as a documented manual step in the release procedure. (F5)
7. **Write the deployment file(s):** image name and its kind, login requirement, pull / start / verify steps, cost of the variant, pass-through variables, state location. Mark any local-build file as the exception it is. (F6, F7)
8. **Bring up the outer edge, then measure from outside** with a client that follows every advertisement the edge makes; disable what does not get through. Repeat after every proxy or network change. (F8)

### Operator

1. Read the header of the deployment file before the first start. (F6)
2. Pull explicitly before every start that is meant to bring a new build. (F7)
3. After every rollout, ask the running instance for its build stamp and compare it with what you meant to roll out. (F7)
4. If you need to pin a build, put the immutable name or the digest into your copy of the deployment file. (F3, F6)
5. If you put a proxy in front of the service, measure from outside what it advertises. (F8)

## Rules

### Builder — MUST

1. Treat the deployment as two roles and six actions — you build, check, publish; the operator pulls, starts, checks — and place every assurance that a push script would provide at the target at one of exactly two places: in the build pipeline **before** publishing, or in the artifact and its deployment file so that the operator can verify it with nothing but the image, the file, and a call against the running instance. Never rely on a check that would need access to the operator's machine. (F1)
2. Before publishing, start the freshly built image in the build environment and require a fixed list of assertions; publish only if all pass. The list contains at least: the entry point starts; the health check the image itself carries reports healthy — wait for that, not for an open port, and abort at once if it reports unhealthy; every interface the deployment file publishes answers on its port; the self-description, where the service has one, is readable without a credential; the in-container user can write to a freshly created state location; a secret the service generates on first start appears in the log exactly once, is accepted, and an unissued one is refused; a **restart** of the container is survived without that secret being generated or logged a second time; the state lives outside the container — proven by **removing** the container and creating a new one from the same state location, after which the same secret still works (a restart alone does not prove this: any state survives a process restart, including state inside the container); and the running instance reports exactly the source revision that was passed into the build. Run every assertion once with its safeguard removed and see it turn red. Use only throwaway values and a throwaway state location — the build log may be public. (F2)
3. Compute image names; never copy a branch name, version string, or any other value from version control into a tag verbatim. Use a computation that knows the registry's tag rules (a tag may not contain a slash, among other things) and sanitizes accordingly. Give every build at least one **immutable** name that carries the source revision (pattern: `sha-<full revision>`) alongside the **moving** names that follow a branch or a version. (F3)
4. Write the default name — `latest` in the common pattern, the one an operator gets who names no tag — from the build of the default branch only: never from another branch, never from a manually triggered build of another branch. It means "the build the builder recommends to anyone who knows nothing else". If the default branch is not that build, do not write the name at all and have the deployment file name a different tag. (F4)
5. Measure visibility instead of inferring it: after publishing and after every change to the visibility setting, fetch the image manifest under the exact name the deployment file uses, with exactly the credentials the operator will have — for a public image, with none, from a step or machine that holds no stored registry credentials. Put that measurement into the release procedure, and into the pipeline as a separate step without the build credential wherever the pipeline can do it. Remember that the source repository's visibility, the image's visibility, and the organization's permission to publish public images are three separate settings. (F5)
6. Ship at least one deployment file per published image, versioned with the source, that **names a published image** rather than building one on the target. Its header states: which image it names and whether that name is moving (follows the builder; a fresh pull gets the new build) or immutable (pins a build; changing it is an edit to the file), and how to switch; whether registry login is needed and, if not, that anonymous access was measured; the steps to pull, start, and verify, in order, with an explicit pull before the start and the version check after it; what travels in clear text in this variant and who it is therefore not for; which variables the operator may set — passed through, never defined empty; and where state lives and that it outlives the container. If several files serve several operating situations, each carries this header. A file that builds locally is a named exception (development, or a target that cannot reach a registry), says so in its first line, names its reason — and that reason is maintained with the file so it does not outlive its truth. (F6)
7. Give the operator a way to ask the running instance for its build stamp, and name that call in the deployment file's header together with what to compare it against (the revision from the build log or the immutable tag). (F7)
8. Measure every capability the outer edge of a deployment advertises to clients — an additional protocol, an additional port, an alternative service (commonly via the `Alt-Svc` header of a reverse proxy) — from outside, at go-live and after every change to proxy or network, with a client that **follows** the advertisement rather than one that ignores it. Switch off at the outer edge whatever the network in front does not let through; do not leave it to clients to fall back. (F8)

### Builder — NEVER

- Never publish an image that has not been started and checked in the build. (F2)
- Never wait for an open port where the image carries a health check; wait for the health check. (F2)
- Never take a container restart as proof that state lives outside the container; only remove-and-recreate proves that. (F2)
- Never run the smoke test with real credentials or a real state location. (F2)
- Never use a branch name, or any other version-control value, as a tag without computing it through the registry's rules. (F3)
- Never write `latest` (or the default name) from a non-default branch or from a manual build of one. (F4)
- Never conclude that an image is public because the repository is public, or because a settings page says so; the proof is the anonymous manifest fetch. (F5)
- Never ship a deployment file that builds on the target as the regular path. (F6)
- Never ship a deployment file that names an image without saying whether the name moves and how to verify what runs. (F6, F7)
- Never leave an advertised protocol or port in place that has not been reached from outside by a client that follows the advertisement. (F8)

### Operator — MUST

1. Read the header of the deployment file before the first start and take from it: which image and which kind of name, whether to log in, what travels in clear text, and how to verify. If the header does not answer these, ask the builder before starting — do not guess. (F6)
2. Pull explicitly before every start that is meant to bring a new build; a container runtime serves a name it already knows from its local cache and does not fetch again on its own. (F7)
3. After every rollout, ask the running instance for its build stamp and compare it with what you meant to roll out — the revision from the build log or the immutable tag. Only then treat the rollout as done. A date, a green mark in the build service, or a successful container restart is not proof of what is running. (F7)
4. If a pull answers "unauthorized" and your credentials are fine, suspect the image's visibility setting, not your credentials; do not create or distribute credentials to get around it. (F5)
5. If you need to pin a build, put the immutable name (or the digest) into your copy of the deployment file and know that from then on a fresh pull changes nothing until you edit the file. (F3, F6)
6. If you put a proxy in front of the service, measure from outside what it advertises and switch off what your network does not let through. (F8)

### Operator — NEVER

- Never treat "the deploy ran" as "the new build is running". (F7)
- Never take `latest` without knowing what the builder promises for it; prefer the name the deployment file gives you. (F4)
- Never build the image locally from the shipped source as your regular way to run it when a published image exists. (F6)

## Acceptance checklist

- [ ] Is there no step in the deployment that writes onto the operator's machine, and does every assurance sit either in the pipeline before publishing or in the artifact and its deployment file? (F1)
- [ ] Does the pipeline start the built image, wait for its own health check, hit every published interface, verify the first-start secret (once in the log, accepted, unissued refused), restart it, remove and recreate it from the same state location, and compare the reported revision with the built one — before the publishing step, with throwaway values only? (F2)
- [ ] Does the smoke test turn red when one of its assertions is removed? (F2)
- [ ] Are all tags computed, does a branch name with a slash produce a valid tag, and does every build get an immutable name carrying the source revision? (F3)
- [ ] Is the default name written by the default branch's build only — and is the default branch actually the build the builder would recommend? (F4)
- [ ] Has the manifest been fetched anonymously under the exact name the deployment file uses, after the last visibility change, from a place without stored credentials? (F5)
- [ ] Does every shipped deployment file name a published image (or say in its first line that it is the local-build exception), and does its header state the kind of name, the login requirement, the pull / start / verify steps, the clear-text cost, the pass-through variables, and the state location? (F6)
- [ ] Can an operator, with nothing but the file and the image, ask the running instance for its build and compare it — and does the header tell them how? (F7)
- [ ] Has every advertisement of the outer edge been reached from outside by a client that follows it, and is everything the network blocks switched off at the edge? (F8)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Tests are green, so the image is published | Start the image in the build and require the assertion list before publishing (F2) |
| Waiting for the port, not the health check | Wait for the image's own health check; a check that never goes healthy must fail in the build (F2) |
| Smoke test with a real password "to be realistic" | Throwaway values and a throwaway state location; the build log may be public (F2) |
| Restarting the container and calling it "state survives" | Remove the container and create a new one from the same state location; a restart proves only that the secret is found again (F2) |
| `tags: repo:${branch}` | Compute tags; a branch with a slash yields "invalid reference format" and no image (F3) |
| Only a branch tag per build | Add `sha-<revision>` so a build can be pinned and compared (F3) |
| `latest` on every push | Enable it for the default branch only — or not at all if that branch is not the recommended build (F4) |
| "The repo is public, so the image is" | Fetch the manifest anonymously from a credential-free place; three settings, not one (F5) |
| Deployment file with `build:` left over from private days | Name the published image; keep local build as a labeled exception (F6) |
| Image named by a moving tag with no word about it | State the kind of name and how to pin (F6) |
| Header says pull/start/read-the-log and stops | Add the verification call and what to compare it with (F6, F7) |
| Restart the container and call it deployed | Pull explicitly, restart, ask for the build stamp, compare (F7) |
| Proxy advertises a UDP protocol the firewall blocks | Measure from outside with a client that follows `Alt-Svc`; disable the advertisement (F8) |
| Hand-tested clients all work, so the server is fine | The hand-tested clients ignored the advertisement; test with one that follows it (F8) |

## Self-check

Before you report the work as done, answer these for yourself:

- Did I act in one role, builder or operator, and apply that role's rules — or both where I am both?
- Is there any check in my result that would need access to the operator's machine? If so, it is not a pull deployment, or the check sits in the wrong place.
- Did I see the smoke test turn red at least once per assertion, or am I only assuming it would?
- Could an operator who has only the deployment file and the image answer, from the header alone, which image, which kind of name, whether to log in, how to pull, start and verify, and what travels in clear text?
- Where my result goes beyond what I could measure (visibility, the outer edge), did I say so instead of claiming it?
- If a rule seems wrong for this case, did I record the deviation and its reason instead of quietly skipping it?
