---
name: service-as-single-artifact
description: Apply when building a small service to put one function on the network — deciding its shape, its command set, its configuration, its packaging and its deployment — or when reviewing such a service for creeping second tools, second images and permission models.
---

# The Service as a Single Artifact — Work Instruction

<!-- SELF-CONTAINMENT RULE: This file is handed to agents on its own. Every rule, step,
     and checklist item is written out in full HERE — never "see F3" or "as defined in
     the proposal". The F numbers in parentheses at the end of items are provenance
     markers for maintainers only (they tie an item to the proposal's decision block for
     sync reviews); an agent must be able to act correctly without ever opening the
     proposal. Sync-review question for every change: does AGENT order what PROPOSAL
     justifies — and is every Fn either covered here or deliberately out of scope? -->

Distilled from the proposal of the same name (German original leading). Orders only —
the reasoning, costs, and limits live in the proposal; nothing here requires reading it.

## When to apply

- Someone wants one function available over the network and asks you to build a small
  service for it.
- You are extending or reviewing such a service and want to keep it from growing a
  second tool, a second image, a management surface or a permission model.
- Note the scope: this is a procedure for building, not an interface design pattern.
  How the finished service describes itself to its consumers is a separate matter.

## Procedure — the eleven steps

Steps 0 to 3 are half an hour of thinking and decide everything that follows. Do not
skip them; you will only redo them later under worse conditions.

**0 · Check whether it must be a service at all.** If the caller sits on the same
machine — a script, a scheduler, an agent on that host — the answer is a command-line
tool, not a service: operating-system permissions are the access control, and network
surface, access artifacts and proxy fall away entirely. Only when the caller comes over
the network, or will later, does the rest of this procedure pay off. *Result:* a
reasoned decision. If it falls against the service, steps 1 to 3 still help and the
rest is unnecessary.

**1 · Write the sentence.** One sentence saying what the service does — with no "and
also". If it will not come out, the cut is not found yet and everything else is
premature. *Result:* the sentence. It later becomes the short description, the first
line of the command help, and the entry someone finds it under. (F1)

**2 · Derive the methods.** The sentence yields the main method; add the helper methods
without which it does not work — preparation (an upload, say), status enquiry, cleaning
up what was prepared. *Result:* the method list. Whatever will not subordinate itself
here does not belong in this service: strike it, or make it a second service. (F1)

**3 · Determine the variant.** What differs from one application case to the next —
target system, storage location, limits? This is the pivot of the whole cut: the answer
is exactly what the configuration entry carries, what the access artifact embodies, and
what the caller therefore never chooses. *Result:* the field list of the variant. (F9)

**4 · Fix the configuration.** One file with the global settings and a list of the
variants from step 3; secrets are not in it but referenced from it. Add the lookup
precedence and the rule that every command shows which file it is using. *Result:*
configuration schema and lookup rule. (F5)

**5 · Name the state locations.** What must survive a restart — data, queues, uploads?
Each gets a named place outside the artifact, sensibly separated per variant.
*Result:* the directory layout. (F6)

**6 · Set up the command tree — before the domain logic.** At minimum: start the
service · check the configuration (optionally with a live test against the target
system) · show state · set up, change and remove a variant · version. Setting up a
variant also issues the access — one command, not two; there is deliberately no
separate "issue access" command, but there is one to renew an existing access. Add the
exit codes and the machine-readable output. The order is deliberate: with the command
skeleton in place, everything else can be checked through commands instead of through
the network surface. *Result:* a runnable skeleton that can do nothing but explain
itself. (F2, F3, F9)

**7 · Build the service mode.** The network surface offers exactly the methods from
step 2; the access check looks up in the configuration what the presented access
embodies; there is no management path and at most a readiness point that says nothing
but "running". *Result:* the service, runnable against a hand-written configuration.
(F1, F7)

**8 · Finish the setup command.** The command created in step 6 now gets its effect: it
configures a variant and in the same motion creates the access for it — show the access
artifact exactly once, keep only its hash and a harmless short identifier, renewal as
its own command. *Result:* the path from "the service runs" to "someone may use it" —
without a secret being left lying anywhere. (F9)

**9 · Package and set up delivery.** Build one artifact whose entry point is the
service mode and against which the commands run in the same environment. More is
decided here than it looks — and whoever builds the service *for someone*, for instance
as an agent for their user, **asks explicitly at this point instead of choosing**:
Should a container image be provided, or does the package or binary suffice for the
target environment? Should the build pipeline be set up along with it, so the image is
built and published automatically for every version? And should the deployment file
then point at that published image — which also settles whether the target machine can
reach the registry and, for a non-public source, with which credentials. The three
questions cost a minute and are expensive later: switching from "builds locally" to
"pulls the published image" touches every deployment site again. *Result:* the artifact
and a delivery directory — container description, deployment file, reverse-proxy
example, unit for the process form and, if wanted, the build pipeline. (F2, F4)

**10 · Establish the operating form.** Local endpoint, reverse proxy in front with the
public name and the certificate; separate users and file permissions so that the
service process reads where the commands write. *Result:* the service is reachable, and
the permission cut sits in the process form instead of in good intentions. (F8, F11)

## Rules

MUST:

1. One service carries exactly one function, describable in one sentence. The network
   surface splits it into a main method and the helper methods it needs; they all serve
   the same function and share the same access. A second, independent function means a
   second service. (F1)
2. Service and commands are the same executable artifact; the mode is chosen at
   invocation. Build no separate administration tool. Further process kinds (worker,
   scheduler) are further modes of the same artifact. (F2)
3. Every command ends with an exit code whose meaning is fixed and documented —
   distinguish at least: all good · runtime error · wrong invocation · configuration
   invalid. Commands that list or show state also offer machine-readable output. No
   command demands interactive input it cannot also take as an argument. (F3)
4. Build and publish exactly one delivery artifact; commands run against it in the same
   environment as the service. The deployment file names that published artifact instead
   of rebuilding it on the target machine; building locally is a named exception. (F4)
5. All modes read the same configuration source with the same code, found by a fixed
   precedence — explicit argument, then environment variable, then user location, then
   system-wide location — and every command shows which file it actually uses. The
   check command anticipates start behaviour exactly. Secrets are not in the file; they
   are referenced from it, in this order: environment variable (a canonical name may be
   proposed) — file with tight permissions used strictly as a pass-through, its content
   never appearing in output, logs or error messages — anything further that does not
   fall below those two properties. If a field is set by several paths, the higher rank
   wins and the check command reports the ambiguity. (F5)
6. Everything that must survive a restart — data, configuration, secrets, queues — sits
   at named places outside the artifact, so a version change carries nothing along and
   leaves nothing behind. (F6)
7. Split state by kind and let permissions follow: **configuration** (what the service
   should do and for whom) is written only by the commands and read by the service
   mode; **operating data** (queues, run state, scratch areas — whatever arises in
   performing the function) is written by the service mode itself, and by the commands
   too, since cleaning up is their job. Enforce this through separate users and file
   permissions in the process form — different directories with different permissions —
   not through intent in the code. (F8)
8. The commands configure the function for the specific application case and create the
   access for exactly that configuration, in one motion. What the configuration fixes,
   the caller never chooses: the access carries it. There is no access without a
   matching configuration, and a change that shifts the variant is a new access. Build
   this in from the start even for a single-operator, single-caller setup: it costs
   nothing there, because the access arises with the configuration anyway, whereas
   retrofitting means converting existing callers, binding targets afterwards and
   inventing a variant where everything used to be global. (F9)
9. Let the service listen on a local endpoint and leave public names, certificates and
   TLS to a reverse proxy in front. The process form — system service unit or container
   runtime — is free; the artifact stays the same. (F11)

NEVER:

- Never build a management surface into the running service: no administration
  endpoints, no debug routes, no management UI. Diagnosis, inspection and change are
  commands, and they need no authentication of their own — server access is the
  authentication. A readiness point that says nothing but "running" is not a management
  surface. (F7)
- Never give the service mode write access to the configuration. (F8)
- Never create administrative rights: no administrative user, no administrative access
  artifact, no permission model for administration. Administration is configuration,
  and whoever may change the configuration administers. No issued access ever carries
  administrative power. (This does not touch a service whose *function* is managing
  accounts or permissions — that it offers over its network surface, because that is
  its function.) (F10)
- Never let the caller choose at runtime what the configuration fixed. (F9)
- Never terminate TLS in the service or listen on all interfaces. (F11)

## Acceptance checklist

- [ ] Can you say in one sentence what the service does, without "and also"? (F1)
- [ ] Do all network methods serve that one function and share one access? (F1)
- [ ] Is there exactly one executable artifact, with the service and all commands in
      it, and exactly one published delivery artifact? (F2, F4)
- [ ] Does the delivery file name the published artifact rather than building
      locally — or is local building marked as a named exception? (F4)
- [ ] Does every command have a documented exit code, and does everything that lists
      offer machine-readable output? (F3)
- [ ] Does every command show which configuration file it uses, and does the check
      command decide exactly as the start would? (F5)
- [ ] Are secrets referenced rather than contained, in the ranked order? (F5)
- [ ] Does a version change carry no state along and leave none behind? (F6)
- [ ] Is there no path in the network surface that has to be blocked in the proxy
      configuration? (F7)
- [ ] Can the service process write operating data but not the configuration, enforced
      by users and file permissions? (F8)
- [ ] Does setting up a variant issue its access in the same command, shown exactly
      once and stored only as a hash? (F9)
- [ ] Is there no access with which one can do more than the one function? (F10)
- [ ] Does the service listen locally, with the proxy holding name and certificate?
      (F11)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| A separate little admin tool beside the service | Make it a mode of the same artifact; two tools mean two configuration parsers with two version numbers (F2) |
| A management web UI "for convenience" | Commands on the server; the surface would need its own login, permissions and error handling, all permanently exposed (F7) |
| Deployment file builds locally (`build:`) | Point it at the published artifact; otherwise there are as many artifacts as machines (F4) |
| Second, "slim" admin image | One image; a second one can carry a different version and see something else than the service (F4) |
| Service process runs as the same user that owns the configuration | Separate users and directories: the service reads configuration and writes operating data only (F8) |
| Caller passes the target as a request parameter | The access embodies the target; a chosen parameter is authorization code that can be wrong (F9) |
| An admin token "for emergencies" | Do not create one; administration is configuration, and it happens on the server (F10) |
| "We will add access tokens later, it is only me for now" | Build them in now: with the access created alongside the configuration it costs nothing, and retrofitting hits when the second caller arrives (F9) |
| A second function added because the service is running anyway | Second service; otherwise callers must be told apart, and roles and a permission model follow (F1) |
| TLS terminated in the service | Reverse proxy in front; otherwise certificate renewals become service restarts (F11) |

## Source

This instruction operationalizes the proposal "Der Dienst als ein Artefakt / The Service
as a Single Artifact" (same directory: PROPOSAL.de.md leading, PROPOSAL.en.md English).
If a rule seems wrong for your case, read the proposal's decisions and limits before
deviating — and record the deviation.
