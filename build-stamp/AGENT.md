---
type: Proposal Agent Guide
name: build-stamp
description: Self-contained guide for an agent that builds an artifact and its build pipeline so that the artifact says which revision it was built from; three steps, the rules, an acceptance checklist and common mistakes.
---

# Agent guide

## Purpose

You are building an artifact and its build pipeline, or you are checking whether a shipped artifact can say which revision of its source it was built from. This guide is complete on its own. Rules are named in parentheses as provenance.

## Procedure

**1 · The version command reports release and build.** When you set up the commands of the artifact, the version command reports the release number and the revision the artifact was built from. (F1)

**2 · Stamp, and check before publishing.** The build stamps the source revision into the artifact itself. The build pipeline starts the built artifact before publishing it and requires that it reports exactly that revision; an artifact that cannot do that is not published. *Result:* the build pipeline with its smoke test. (F1)

**3 · The first look after deploying.** Run the version command against the running instance; it reports the revision that was meant to be deployed. *Result:* what is running is demonstrably what was built. (F1)

**Decide the choices.** This concept has no choices.

## Rules

MUST: the build stamps the source revision into the artifact itself, not into a companion file and not into the artifact's name; a release number alone is not enough, because it does not move between two builds · the version command reports that stamp and is run against the running instance, inside the running container or, on the host, as the same binary (where it reports the installed build; only a restart after installation guarantees the running process is the same) · the build pipeline starts the built artifact before publishing it and requires that the reported revision is exactly the one built · a locally built artifact says "unknown" rather than inventing a revision · the readiness point still says nothing but "running"; if any network surface reports the build, it is authenticated. (F1)

NEVER: publish an artifact that cannot report the revision it was built from. (F1)

## Acceptance checklist

- [ ] Does the artifact carry its source revision, does the version command report it against the running instance, and does the build refuse to publish an artifact that does not? (F1)
- [ ] Does a locally built artifact say "unknown"? (F1)
- [ ] Does the readiness point say nothing but "running", and is every network surface that reports the revision authenticated? (F1)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Image tagged with a branch name and nothing else | A branch name is a pointer, not a revision; stamp the source revision into the image and have the version command report it (F1) |
| "Deployed" taken as "running" | An unchanged tag pulls nothing new; ask the running instance for its build with the version command (F1) |

## Self-check

Before you report completion: can you name the revision the running instance was built from, and say how you asked it? If not, you are not done.
