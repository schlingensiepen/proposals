---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: service-as-single-artifact
language: en — translated from the German original
sync: v1.0 (2026-08-15)
question: "I want to put a function on the network and am building a small service for it — which rules do I take as my foundation so that it stays small and is still safe to operate?"
thesis: "A service carries exactly one function, is built as one artifact with several modes and shipped as one delivery artifact; the commands configure the function for the specific application case and create the access for exactly that configuration — which makes roles, permission management and an administrator unnecessary before they arise."
---

# The Service as a Single Artifact

**In one sentence:** Whoever wants to put a function on the network builds a service for it that carries exactly that one function, as *one* artifact with several modes (running and commands), ships exactly one delivery artifact, and lets the commands configure the function for the specific application case and, in the same motion, create the access to it.

**Abstract:** Something is to go on the network — a function that other people or their agents should be able to use — and one quickly builds a small service for it. Just as quickly, a second thing grows up around it: an admin script, a management UI, a debug endpoint, a second image; and with the second function come roles, permissions and an administrator. This dossier is the starter set against that: eleven rules one adopts before the first line is written — one function, one artifact, one delivery artifact, one access that comes into being together with its configuration.

## 1. Problem

One has a function that is to go on the network — something other people or their agents should be able to use. The author faced exactly this case: drafts were to be filed into mailboxes over IMAP, invoked from outside. For something like that one does not build an enterprise service but a small program — and one wants to have it in operation before the appetite fades.

Then come the questions that arrive within a few days for every service, however small: How does one create an access? Who may use it, and for what exactly? How does one check whether the configuration is right *before* restarting? How does one see what is pending? How does one clean up? And how does TLS get in front of it?

The reflex says: another tool. A little admin script that writes into the same store. A web UI for administration. A debug endpoint, "internal only". A second, slim image for maintenance tasks. Each of these things looks sensible on its own, and together they turn the small service into a project — for each is a second implementation of the same knowledge: it has to know the configuration format, the storage layout, the validation rules, the version. From then on two things age side by side, and at some point the tool from version 1.2 writes something that the service from version 1.5 reads differently.

The second reflex is quieter and more expensive: another function. If the service is running anyway, it can surely solve the neighboring problem at the same time. From that moment on there are callers who are to be allowed one thing and not the other — and with them permissions, roles, an administration for them and someone to run it. The service that began as an afternoon project now has a permission concept.

Three roles carry the problem, and in a small service the first two often coincide in one person — that is the normal case this dossier is written for. **The builder** writes both and notices the duplication first at the third configuration field that has to be carried over in two places. **The operator** has to ship two delivery artifacts, keep two versions in view and, in a fault, know which of the two is currently right. **The successor** finds, months later, a tool whose relation to the service nobody can explain any more — and does not dare to use it.

The usual way out — discipline, shared libraries, a monorepo — mitigates the duplication but does not abolish it: two delivery artifacts remain two delivery artifacts, and they are built separately, shipped separately and forgotten separately. The structural answer starts one level deeper: what does not exist separately cannot age separately — and what is not ambiguous in the first place needs no permission management.

This dossier is therefore not a doctrine of architecture but a **foundation for starting to build**: a procedure in eleven steps (§2) and eleven rules that carry it (§4) — both are adopted before the first line is written. Whoever begins this way has already made the decisions one otherwise makes one by one and mostly under time pressure — and is spared exactly the by-products that make the small service big.

## 2. The concept

Everything begins with the function: **a service carries exactly one.** It is its purpose, describable in one sentence — "put a mail into an IMAP folder". The network surface splits it into methods: a main method and the **helper methods** without which it does not work. In the example, the file upload is not a second function but the preparation step for attachments; added to it are the status enquiry about the submission and the cleaning up of what was uploaded. All methods serve the same function and share the same access — there is nothing that one caller would be allowed to do and another not.

From this follows the construction form: **one artifact with several modes.** The same executable program runs as a service and executes the commands — check, show state, configure, issue an access, clean up; which mode applies is decided by the invocation. If the service needs further process kinds — a worker for a queue, say — that too is a mode and not a second program. What is shipped is **one delivery artifact**: an image or a binary, against which the commands run as well, in the same environment, with the same version. Both modes read **the same configuration** with the same code, and the check command passes exactly the judgment that the start would pass. Whatever must survive a restart lies **outside** the artifact, at named places.

The core of the cut, however, sits in what the commands do: **they configure the function for the specific application case and create the access for exactly that configuration.** The service can "put a mail into an IMAP folder"; the command determines into *which* mailbox and into *which* folder — and in the same motion issues the access artifact that embodies exactly this variant. The caller never chooses what the configuration has determined; they invoke the function, and where it takes effect is not in their request.

That is the reason the rest turns out so small. There is no moment at which someone *gives* rights to an existing access — the granting is the configuration, and the access is its handle. So this service needs **no permission model, no roles and no administrator**: administration is configuration, and whoever may change the configuration file administers. Accordingly, the **running service has no management surface** — no management path, no debug route — and the **permission cut runs between the modes**: the service mode works with the narrowest permissions sufficient to run; writing happens only in command mode, enforced through users and file permissions.

The principle is not new; it stands in the ancestry of the twelve-factor rule that admin processes run in an environment identical to the service's and that their code must ship with the application code (§10). What is new is the consistency with which a cut is made of it here: not "admin code lies with the application code" but "admin code *is* the application code, in a different mode" — and the connection with the one function, which dissolves the permission topic from the outset. For whoever wants to put a function on the network, that is the whole effort: one program with a few more commands, one delivery artifact, one configuration file, one directory beside it, one proxy in front.

### The procedure

The decisions in §4 say what must hold. This section says in which order one proceeds and what is on the table at the end of each step — that is the actual utility for whoever wants to get going. Steps 0 to 3 are half an hour of thinking and decide everything that follows; whoever skips them makes up for them later under worse conditions.

**0 · Check whether it must be a service at all.** Whoever invokes the function either sits on the same machine or somewhere else. If they sit there — a script, a scheduler, an agent on the same machine — then the answer is a command-line tool and not a service: the access control is provided by the permissions of the operating system, and network surface, access artifacts and proxy fall away entirely. Only when the caller comes over the network, or is to come later, does the rest of this procedure pay off. *Result:* a reasoned decision. If it falls against the service, steps 1 to 3 are still useful and the rest is unnecessary.

**1 · Write the sentence.** One sentence that says what the service does — with no "and also". If it will not come out, the cut is not found yet, and everything else would be premature. *Result:* the sentence. It later becomes the short description of the service, the first line of its command help, and the entry under which someone finds it again. (F1)

**2 · Derive the methods.** From the sentence follows the main method; added to it are the helper methods without which it does not work — preparation (an upload, say), status enquiry about the submission, cleaning up what was prepared. *Result:* the method list. Whatever will not subordinate itself here does not belong in this service: it is struck, or it becomes a second service. (F1)

**3 · Determine the variant.** What differs from one application case to the next — target system, storage location, limits? This question is the pivot of the whole cut: its answer is exactly what the configuration entry later carries, what the access embodies, and what the caller consequently never chooses. *Result:* the field list of the variant. (F9)

**4 · Fix the configuration.** One file with the global settings and a list of the variants from step 3; secrets are not in it but stand there as a reference to an environment variable or a file. Add the lookup precedence and the rule that every command shows which file it is using. *Result:* configuration schema and lookup rule. (F5)

**5 · Name the state locations.** What must survive a restart — data, queues, uploads? Each of them gets a named place outside the artifact, sensibly separated per variant. *Result:* the directory layout. (F6)

**6 · Set up the command tree — before the domain logic.** At minimum: start the service · check the configuration (optionally with a connection test against the target system) · show state · set up, change and remove a variant · version. Setting up a variant also issues the access — one command, not two (F9); there is deliberately no separate "issue access" command, but there is one for renewing an existing access. Add the exit codes and the machine-readable output. The order is deliberate: with the command skeleton finished, everything else can be checked through commands instead of having to be tested through the network surface. *Result:* the runnable skeleton that can do nothing yet but explain itself. (F2, F3, F9)

**7 · Build the service mode.** The network surface offers exactly the methods from step 2; the access check looks up in the configuration what the presented access embodies; there is no management path and at most a readiness point that says nothing but "running". *Result:* the service, runnable against a hand-written configuration. (F1, F7)

**8 · Finish the setup command.** The command created in step 6 now gets its effect: it configures a variant and in the same motion creates the access for it — show the access artifact exactly once, keep only its hash and a harmless short identifier, renewal as its own command. *Result:* the path from "the service runs" to "someone may use it" — without a secret being left lying anywhere. (F9)

**9 · Package and set up the delivery.** Build one delivery artifact whose entry point is the service mode and against which the commands run in the same environment. More is decided here than it looks — and whoever builds the service *for someone*, as an agent for their user, say, **asks explicitly at this point instead of choosing**: Should a container image be provided, or does the package or the binary suffice for the target environment? Should the build pipeline be set up along with it, so that the image is built and published automatically for every version? And should the deployment file then point at that published image — which also settles whether the target machine can reach the registry and, for a non-public source, with which credentials. The three questions cost a minute and are expensive later: switching from "builds locally" to "pulls the published image" touches every deployment site once more. *Result:* the artifact and a delivery directory — container description, deployment file, example for the reverse proxy, unit for the process form and, if wanted, the build pipeline. (F2, F4)

**10 · Establish the operating form.** Local endpoint, reverse proxy in front with the public name and the certificate; separate users and file permissions, so that the service process reads where the commands write. *Result:* the service is reachable, and the permission cut sits in the process form instead of in good intentions. (F8, F11)

What this procedure deliberately does not prescribe: language, framework, data storage, interface dialect. It fixes the order and the results, not the means.

## 3. Terms

**Function** — the one thing the service can do, describable in one sentence; its purpose.

**Main method** — the call that triggers the function.

**Helper method** — a call without which the function does not work or is not verifiable: preparation (file upload, say), status enquiry, cleaning up. Helper methods are not functions of their own and share the access of the main method.

**Artifact** — the one built, versioned, shippable delivery artifact of a service: the binary or the container image.

**Mode** — an operating mode of the same artifact, chosen at invocation.

**Service mode** — the long-running mode that serves the network surface.

**Command** — a one-off invocation of the same artifact for configuration, checking, state display or cleaning up; started locally, ends when its task is done.

**Access** — the artifact with which a caller uses the function (as a rule a token); it embodies a concrete, preconfigured variant of the function.

**Management surface** — any possibility, reachable over the network, of configuring the service, inspecting it or changing accesses; not provided for in this concept.

**State** — everything that must survive a restart: data, configuration, secrets, queues; lies outside the artifact.

**Process form** — how the artifact is started and supervised: systemd unit, container runtime, something else; interchangeable for this concept.

**Reverse proxy** — the instance placed in front that serves public names and TLS and passes on to the local endpoint of the service.

**Check command** — the command that assesses the configuration without starting the service.

**Version drift** — the state in which two separately built delivery artifacts of the same system carry different versions of the same knowledge.

## 4. Decisions

**Map.** Eleven decisions in three blocks. **F1–F6** describe *the cut*: the one function, the one artifact with its modes, the contract of the commands, the one delivery artifact, the one configuration and the state beside it. **F7–F10** draw *the separation*: no management surface in the running service, a permission cut between the modes, an access that comes into being with its configuration — and therefore no administrative rights at all. **F11** concerns *the outer edge* in operation. F1 carries everything: with the second function, permissions, roles and administration return, and F9 as well as F10 lose their ground. Every decision names who it applies to.

### F1 · One service, one function

**Applies to:** Builder.

**What holds.** The service carries exactly one function; it is its purpose and describable in one sentence. The network surface splits it into methods: a main method and the helper methods without which it does not work (preparation, status enquiry, cleaning up what was prepared). All methods serve the same function and share the same access. Whoever needs a second, independent function builds a second service.

**Why.** The one function is the precondition for everything else staying small: a service with exactly one function needs no distinction as to who may use which method — and hence neither roles nor permission management nor someone to run them (F9, F10). It is moreover describable in one sentence, for humans as for agents, and its description stays readable instead of turning into a catalog.

**Obvious and rejected.** *One service for everything ("since it is running anyway")* — as soon as two independent functions lie under one access, a permission distinction is needed, and with it roles and their administration; saving one delivery does not outweigh that burden. *One access per method* — then the caller juggles several accesses in order to upload a file and use it; the ability is the function, not the individual call. *The helper methods as small services of their own* — distributes one workflow across several deliveries and produces exactly the coordination problems this dossier avoids.

**Symptom of violation.** One is discussing whether a caller should be allowed one method and not the other — or the service can no longer be described in one sentence without saying "and also".

### F2 · One artifact, several modes

**Applies to:** Builder.

**What holds.** Service and commands are the same executable artifact; the mode is chosen at invocation. Present at a minimum are: the **service mode**; a **check command** that assesses the configuration without starting; a **state command** that shows what is pending; **configuration commands** for setting up and changing; and the command that **issues an access**. Further process kinds (worker, scheduler) are further modes of the same artifact. A separate administration tool is not built.

**Why.** The commands share everything essential with the service: configuration format, storage layout, validation rules, data schema, version. A second tool is a second implementation of this knowledge — and two implementations of the same knowledge drift apart as soon as one of them changes. Within the same artifact they cannot. The minimum set is named explicitly, because without it the cut would remain an attitude: every service that runs longer than a week needs these five things.

**Obvious and rejected.** *A separate little administration tool* — allegedly cleaner, in fact a second configuration parser with a version number of its own. *An administration web UI* — convenient, but a permanently open attack surface with a login of its own (F7). *Scripts that write directly into the store* — bypass every check the service applies when writing, and know the structure only by hearsay.

**Symptom of violation.** Administration tool and service carry different version numbers — or a new configuration field has to be carried over in two places.

### F3 · Commands are scriptable

**Applies to:** Builder.

**What holds.** Every command ends with an exit code whose meaning is fixed and documented; distinguished at a minimum are: all good · runtime error · wrong invocation · configuration invalid. Commands that list or show state additionally offer a machine-readable output. No command demands an interactive input that it does not also accept as an argument.

**Why.** Administration without a surface has to be automatable — otherwise the second tool comes about through the back door: as a collection of scripts that take apart text outputs and break silently at every format change. Exit codes are the cheapest contract a program can offer, and the only one a deployment chain understands without assistance.

**Obvious and rejected.** *Output text only ("one can search it, after all")* — makes every improvement of the format a break of someone else's automation, and a soundless one at that. *Interactive prompts without a non-interactive path* — then the command hangs in every unattended environment. *Everything over one collective exit code* — the difference between "invoked wrongly" and "configuration broken" is exactly the one an operator needs at three in the morning.

**Symptom of violation.** In the deployment chain there is a text search on a command output — or a command waits for an input in an unattended environment.

### F4 · One delivery artifact, one delivery

**Applies to:** Builder · Operator.

**What holds.** Exactly one delivery artifact is built and published. Commands run against this delivery artifact in the same environment as the service — in container operation as a one-off invocation against the same image, on the host as the same binary. There is no second, "slim" administration image. Part of the delivery artifact is **how it comes into being and how it arrives**: a build pipeline that produces and publishes it reproducibly from the source, and a deployment file that **names** exactly this published delivery artifact instead of rebuilding it on the target machine. If it is built locally after all — in development, or because the target machine cannot reach a registry — that is a named exception and is marked as such in the delivery directory.

**Why.** A second delivery artifact can have a different version, different libraries, different paths, a different time zone — and then the command sees something other than the service does. The identical environment is not a convenience but the precondition for a check result saying anything about the service at all. The same argument carries all the way into the deployment file: whoever has it build locally ends up with as many delivery artifacts as machines — each with the state of the base layers from the day it was built. "One delivery artifact" is then no more than an intention.

**Obvious and rejected.** *An administration image of its own without the service's dependencies* — saves megabytes and buys them with version drift. *The deployment file builds locally* — convenient the first time, after that the multiplication of the artifact: two machines with the same version number can behave differently, and nobody sees why. Fine as a development convenience, not as a delivery path. *Administration as a sidecar container* — the same separation, only with more moving parts. *Commands from the developer's machine against the production store* — a different environment, a different version, and the only protection is attentiveness.

**Symptom of violation.** Someone has to ask which version the administration tool has — or a command behaves differently on the server than locally — or two machines carry the same version number and behave differently, because each built its image itself.

### F5 · One configuration, one judgment

**Applies to:** Builder.

**What holds.** All modes read the same configuration source with the same code. The lookup follows a fixed precedence — explicit statement at invocation, then environment variable, then user-level location, then system-wide location — and **every command shows which file it actually uses**. The check command anticipates start behavior exactly: what it accepts starts; what it rejects does not start. Secrets are not in the file; they are referenced from it, and in this ranked order: **1.** environment variable — the service MAY propose a canonical name for it; **2.** if the environment does not allow that: a file with tight permissions, used exclusively as a pass-through — the content flows directly into the call and never appears in outputs, logs or error messages; **3.** anything further (a secret manager of the environment, say) is permissible as long as it does not fall below these two properties. If a field is set by several paths, the higher rank wins, and the check command reports the ambiguity. The sister concept (§10) justifies this ranked order in detail; it can be followed without it.

**Why.** A check that does not make the same decision as the start is worse than none: it creates trust that breaks in the fault case. The duty to display sounds like a trifle and is the most frequent time-waster in fault-finding — two configuration files on one machine, and one reliably changes the wrong one. And a file can be checked, versioned and rolled back as a whole; scattered environment variables cannot.

**Obvious and rejected.** *A separate schema-checking tool* — the second truth about the configuration, see F2. *All configuration over environment variables* — widespread, but without an overall check and without versionability; for secrets it remains the right way, for the rest it does not. *Checking by trial start* — binds ports and state and is not repeatable in running operation.

**Symptom of violation.** The service does not start although the check was green — or someone changes a file for a long time that is not read at all.

### F6 · The state lies outside the artifact

**Applies to:** Builder · Operator.

**What holds.** Everything that must survive a restart — data, configuration, secrets, queues — lies at named places outside the artifact. The artifact is interchangeable: a change of version takes nothing along and leaves nothing behind.

**Why.** Otherwise "one delivery artifact" is an assertion: an update would destroy state, a rollback would lose data, and nobody could take a version back without danger. The interchangeability of the artifact is the practical return on the whole cut.

**Obvious and rejected.** *The data store in the image* — convenient at the first start, fatal at the second. *Compiling the configuration in* — every change becomes a rebuild, every environment its own delivery artifact (F4). *State in the working directory without a named place* — works until someone changes the process form.

**Symptom of violation.** A change of version requires a data migration — or a rollback loses something that was there before.

### F7 · The running service has no management surface

**Applies to:** Builder.

**What holds.** The service mode offers no administration endpoints, no debug routes and no management UI. Checking, inspection and change are commands. These need **no login of their own**: they are started locally by someone who already has server access — this access *is* the authentication. Not meant are pure operational endpoints without effect and without information about contents, a readiness point, say, that says nothing but "running"; where their boundary runs is an open question (Q3).

**Why.** A management surface on the network is reachable around the clock, whereas a command runs only when someone starts it. It needs a login of its own, a permission check of its own and error handling of its own — three new places where something can be wrong, for a task that presupposes server access anyway. And it is the most convenient path for an attacker who already has the network surface.

**Obvious and rejected.** *A management path behind a simple login* — the weakest login of the system guards its strongest effect. *A debug endpoint "internal only"* — "internal" is a network assumption, not a property. *Runtime switching of behavior over the network surface* — makes the service configurable from outside and the configuration file a fiction (F5).

**Symptom of violation.** In the reverse-proxy configuration a path has to be blocked explicitly — or a block is missing, and nobody notices.

### F8 · The permission cut runs between the modes

**Applies to:** Builder · Operator.

**What holds.** The state falls into two kinds, and the permission cut follows this division. **Configuration** — what the service is to do and for whom — is written exclusively by the commands; the service mode only reads it. **Operating data** — queues, run state, scratch areas, everything that arises in performing the function — is written by the service mode itself; the commands may write there as well, for cleaning up and intervening is their job. The service mode otherwise runs with the narrowest permissions sufficient to run. Both are enforced through separate users and file permissions in the process form, not through intent in the code: the configuration location and the operating-data location are different directories with different permissions. Which permissions exactly are needed per mode depends on the service and is to be fixed on adoption (Q1).

**Why.** A compromised network surface inherits exactly the permissions of the service process. If that process cannot write the **configuration**, neither can the attacker — they can misuse the function but not repurpose it, cannot issue accesses and cannot shift a target; out of the takeover comes limited damage. For the **operating data** this does not hold, and it would be pointless as well: the service has to write there in order to do its work. The separation is therefore not a hardening against everything but one against repurposing — and that is precisely the expensive case. A separation enforced in the code does not help with it: whoever controls the process also controls its checks.

**Obvious and rejected.** *Both modes as the system administrator* — convenient, and the cut is worthless. *One service user with full access to everything* — the same convenience, only with a better conscience; with it the separation between configuration and operating data collapses. *Configuration and operating data in the same directory* — then the permissions can no longer be distinguished without administering every file individually. *Separation in the code only ("the service never calls the write function")* — a declaration of intent that an attacker does not read.

**Symptom of violation.** The service process is allowed to write the configuration file — or configuration and operating data lie so close together that nobody can say who may change what.

### F9 · Configuration and access come into being in one act

**Applies to:** Builder · Operator.

**What holds.** The commands configure the function **for the specific application case** — which target system, which storage location, which limits — and create the access for **exactly this configuration**; both are one motion. What the configuration determines, the caller never chooses: the access carries it. There is no access without a matching configuration, and a change that shifts the variant is a new access.

**Why.** Here the permission question disappears before it arises. Whoever configures, grants; what has been granted stands in the configuration; the access is merely the handle on it. There is no point in time at which someone *adds* something to an existing access — and hence nothing to administer either (F10). Incidentally, one check in the service falls away: the caller cannot name the target at all, so nobody has to check whether they would be allowed to. **This holds expressly for single-operator use as well**, where operator and caller are the same person and an access artifact seems superfluous at first: it costs nothing there, because it arises along with the configuration anyway — there is no step one could save. Retrofitting, by contrast, is expensive: then existing callers have to be converted, targets have to be bound to accesses after the fact, and a variant has to be invented where everything used to be global — and experience shows that this happens exactly when the second user arrives and there is no time. Built in from the start, the cut scales upward without ever having to be rebuilt: the step from the first caller to the second is then a single command invocation.

**Obvious and rejected.** *First create the access, then configure* — creates the intermediate state "access without a cut", and it is exactly in this in-between space that permission management arises. *Making the target selectable at runtime ("the caller says where")* — shifts the responsibility to the caller and forces the service into a check it would not need without this choice. *Self-registration with subsequent release* — sensible for services that want to take agents on without the operator lifting a finger (sister concept, §10); unnecessary here, because the operator configures anyway, and expensive, because it introduces exactly that intermediate state.

**Symptom of violation.** One is discussing which permissions an existing access should get.

### F10 · The service knows no administrative rights

**Applies to:** Builder.

**What holds.** There is no administrative user, no administrative access and no permission model for administration. Administration is configuration, and whoever may change the configuration administers — enforced through file permissions (F8). No issued access ever carries administrative power; it carries the function, nothing else. Not meant is the domain model of a service whose *function* is the administration of accounts or permissions: what it offers as its function, it offers over its network surface (F1).

**Why.** A permission model is expensive — it wants to be designed, checked, documented and maintained in operation — and for a service with one function and preconfigured accesses it has no object. Where there are no administrative rights, nobody can inherit them, extend them or forget to withdraw them.

**Obvious and rejected.** *An administration token "for emergencies"* — precisely the artifact whose loss costs everything, and it is guaranteed to be lying around somewhere. *Roles in the service for operational tasks* — introduces the very permission management this cut saves, and turns the network surface back into an administration path (F7).

**Symptom of violation.** There exists an access with which one can do more than the one function.

### F11 · No TLS in the service, local endpoint

**Applies to:** Builder · Operator.

**What holds.** The service listens on a local endpoint; public names, certificates and TLS are served by a reverse proxy in front of it. The process form — systemd unit or container runtime — is freely selectable; the artifact stays the same.

**Why.** Certificate procurement and renewal are solved problems, but not solved anew in every service: a built-in certificate client ties renewals to restarts of the service and makes every version a security event. And a service that listens only locally has no external surface at all without a proxy — the exposure is a deliberate act, not a default state.

**Obvious and rejected.** *TLS in the service* — a certificate client of its own, renewal logic of its own, restarts at every certificate change. *Listening directly on all interfaces* — the exposure becomes an oversight instead of a decision.

**Symptom of violation.** The service has to be restarted because a certificate was renewed.

**Load-bearing structure (brief recapitulation).** F1 carries everything: with a second independent function, permission distinction, roles and administration return, and F9 as well as F10 lose their ground. F2 carries the tooling part; without "one artifact", F4 (one delivery artifact) and F5 (one judgment) are not enforceable but mere intentions, and F3 would have no place. F6 is the condition for F4 being worth anything in operation — an artifact that drags its state along is not interchangeable. F7 and F8 are the security separation that this cut makes possible in the first place: without a command mode, administration would have to go somewhere, and the only remaining place would be the network surface. F9 and F10 hang together — if F9 falls, the intermediate state arises in which administrative rights are needed, and F10 falls with it. F11 carries independently and holds also for services that adopt nothing else from this concept.

## 5. Security considerations

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Takeover via the network surface | a vulnerability in the service mode is exploited | The process cannot write the configuration, hence cannot repurpose the function (F8); there is no administration function that could be reached (F7); and no access that would carry more than the one function (F10) |
| Privilege escalation via a captured access | a valid access artifact falls into the wrong hands | The access carries exactly the preconfigured variant of the function; target and scope are not in the request and cannot be shifted (F9) |
| Management surface as a way in | management path or debug route with a weak login | There is none: administration is a locally started command whose authentication is the server access (F7) |
| Version drift of the administration tool | a tool from an older version writes into the store | One artifact, one version (F2); commands run against the same delivery artifact (F4) |
| Administration from a foreign environment | a command from the developer's machine against the production store | Commands only in the same environment as the service (F4); write permissions only there (F8) |
| Secret in the artifact | configuration with credentials built into the image | State and secrets lie outside (F6); the configuration file contains only references to secrets, not the secrets themselves (F5) |
| Existence oracle | systematic guessing at foreign resources | Foreign resources answer identically to nonexistent ones; the access limits the view to its own variant anyway (F9) |
| Outdated TLS library in the service | the service terminates TLS itself and is rarely updated | TLS lies with the reverse proxy, which is built and maintained for it (F11) |
| Unintended exposure | the service listens on all interfaces | Local endpoint; the exposure is a deliberate proxy configuration (F11) |

Limit of the model: against an attacker who already has server access, nothing here protects any longer — the commands then stand as open to them as to the operator, and that is intended: their authentication *is* the server access. Whoever cannot make this assumption will find in §7 the conditions under which the cut tips.

## 6. What it costs

This section enumerates what the cut costs. Every point is pinned to the decision that causes it, and stands here without a counter-calculation.

**The second function costs a second service (F1).** What suggests itself — "it can do that at the same time, surely" — is forbidden here, and the price is real: a second delivery artifact, a second delivery, a second store, a second entry in the proxy. Whoever has many small functions administers many small services; the saving lies inside each individual one, not in their number.

**The artifact carries code the service never executes (F2).** The commands lie in the same delivery artifact — they enlarge it, they extend the attack surface of the *image* (though not that of the running process), and they have to be maintained, tested and shipped along with it.

**The delivery chain wants maintaining (F4).** Build pipeline, registry, credentials of the target machine, and, for a non-public source, the question of who may pull the image at all: for an afternoon project this is the part that easily looks bigger than the service itself. Whoever forgoes it and lets the target machine build saves this work — and pays with the multiplication of the artifact that F4 is meant to prevent.

**Command-line ergonomics have to be built (F2, F3).** An administration nobody likes to operate gets bypassed — with scripts that write directly into the store. So there is a need for expressive commands, usable outputs, reliable exit codes, a machine-readable form and a help text one understands without a manual. That is work which would be more visible in a web UI and is therefore often done more readily.

**Operational tasks need server access (F7).** Checking the configuration, issuing accesses, cleaning up the inventory: for that there is no surface but the command line on the server. For the small service whose builder and operator are the same person, that is a saving and not a loss. If operations grow and an on-call or support team is to take on such tasks, they need system access — or a management surface is needed after all, with everything attached to it. The domain side is untouched by this: what the service offers as its function remains reachable over its network surface.

**Every change of the variant is a new access (F9).** If the same application is to write into a different folder in future, one field is not changed; instead an access is newly issued and the old one withdrawn — with everything that means at the caller's end. The stiffness is intended; comfortable it is not.

**The permission cut costs care in the deployment (F8).** Separate users, file permissions per directory, a command invocation under the right user — configuration work in the process form, error-prone and to be performed anew in every environment.

**The reverse proxy is an additional component (F11).** It has to be selected, configured, updated and taken into account in the fault case; fault-finding gains one more station.

**State outside means administering state (F6).** Named places, permissions, backups, restoration: what the artifact does not bring with it, operations must provide. The price of interchangeability is that there is something that is not exchanged.

## 7. Scope and limits

### Preconditions

**The task can be captured as one function.** There must be a sentence that says what the service does, without "and also" (F1). You recognize the situation in the attempt to write it.

**Whoever administers the service has server access.** The commands forgo a login of their own because they presuppose it (F7). Meant are operational tasks on the service, not its domain use. You recognize the situation by who is to issue accesses and change the configuration — if those are people other than those responsible for the system, the assumption tips (below).

**The process form allows one-off invocations in the same environment.** Whether systemd, container runtime or a plain binary: it must be possible to execute the same delivery artifact briefly and locally (F4).

**State can be stored outside.** Named directories, a volume, external storage — anything that survives the change of version (F6).

### When the answer tips

**The service must carry several independent functions.** If two things necessarily share the same state and cannot be cut into two services, the permission distinction returns: then accesses of different kinds are needed, a rule as to who gets which — and with it the very administration that F9 and F10 abolish. You recognize the situation by the fact that one variant of the access is to permit one method and not the other.

**Operational tasks are to be performed without system access.** Meant are tasks on the service itself — issuing accesses, changing the configuration, cleaning up the inventory — that people without server access are to take on: on-call, support, domain administration. The answer tips because F7 draws its simplicity precisely from the server access being the authentication; otherwise one would have to give many people system rights in order to give them operational tasks. Then an authenticated management surface is needed after all — and with it the login, permission and evidence questions that F7 avoids. You recognize the situation by the question "Can somebody do that without server access?", and by the fact that it is asked regularly. Not meant is the domain use: that users use the function over the network is the purpose and not a tipping condition.

**The platform does not allow a one-off invocation.** Some operating environments permit only the rollout of long-running processes, no access to a running instance. Then F4 cannot be redeemed; the commands would have to be rolled out as a separate, short-lived process of the same image — feasible, but the cut becomes cumbersome, and one should know that beforehand.

**The service consists of several runtimes.** If parts are built in different languages, there is no longer "one binary". Then the principle holds only per component, and the question of the common administration path arises anew (Q2).

**Duty of evidence per person.** Where it must be proven who performed which operational action and when, "had server access" is not enough; the process form has to provide it, or the management surface returns.

### What it does not solve

**Scaling and failure tolerance.** Whether a service runs in several instances, how loads are distributed and failovers arranged, this concept does not say; it only makes the artifact interchangeable enough for such procedures to remain possible.

**Data migrations.** When and how schema changes are carried out — at the start or as a command — is open and is handled controversially (Q4).

**Observability and backup.** Logs, metrics, alerts, data backup and restoration remain operational work outside this cut.

**The domain user and role model.** Users, roles and grants *within* the function of a service run crosswise to this cut: it neither forbids them, nor prescribes them, nor replaces them. Accesses are here the one defined way in from outside; how a service models things internally remains its own business. This dossier says only that the *administration of the service* needs no such model (F10).

## 8. Track record

This section shows what has been built and separates it from what is still outstanding. Unlike in the sister dossiers, the running example here is **not invented**: the service is specified exactly according to this cut, which is why it serves at once as evidence and as a walk-through from beginning to end. All the values named come from its short specification. What is thereby evidenced is the construction cut; operational experience it is not yet. **And a warning up front:** the concrete names and values here — command designations, token prefix, configuration format, the worker strand for the queue — are *illustration of this one service*, not a house convention. Binding are the procedure from §2 and the decisions from §4; whoever copies this section instead of reading it replicates apparatus that their own function may not need at all.

### The draft-filing service, from beginning to end

**One function (F1).** The service puts a mail into an IMAP folder — that is all. The main method receives the message (`POST /messages`, answer `202` with a submission identifier); the helper methods are the file upload for attachments and HTML bodies (`POST /files`, then `GET`/`DELETE` on the file), the status enquiry about the submission (`GET /messages/{id}`) and a readiness point (`GET /healthz`). All share the same access. Here too, what the service does *not* do is remarkable: it holds nothing, searches nothing, offers no retrieval of filed mails — keeping and findability are provided by the mailbox, and every mail client can access it. A storage of its own beside it would be the second source with all the consequences that brings.

**One artifact with modes (F2).** There is exactly one program. `serve` starts the service; `check` checks the configuration and can additionally test the connection to the target system with `--live`; `status` shows local metrics and queue state; under `config` lie setting up, listing and changing sites and accounts including `rotate-token`; `queue` and `files` allow explicit intervention in the queue and the file pool; plus `version`. The worker strand for message delivery belongs to the same program.

**Scriptable commands (F3).** The exit codes are fixed: 0 all good · 1 runtime error · 2 wrong invocation · 3 configuration invalid · 4 live test failed. Listing commands and `status` offer `--json`.

**One delivery artifact (F4).** What is shipped is a Python package and a container image (`ENTRYPOINT ["imap-service","serve"]`, non-root, readiness check on the health endpoint) together with a `deploy/` directory containing a Dockerfile, a compose file, and proxy and systemd configuration. Commands run against the same image.

**One configuration, one judgment (F5).** The configuration is a TOML file; it is looked for in the order `--config-file` → `$IMAP_SERVICE_CONFIG` → user-level location → `/etc/imap-service/config.toml`, and the commands show which file they use. Reading is done with the standard library, writing is format-preserving, validation gives field-precise error messages. Mailbox passwords are not in the file but stand there as a reference (`env:VAR` or `file:/path`).

**State beside it (F6).** One data directory per site and access, with separate areas for uploaded files and queue state. A change of version swaps the image; the store remains.

**No management surface (F7).** The network surface knows no management path. The only unauthenticated point is `GET /healthz`, and it answers exclusively with `{"status":"ok"}` or 503 — no metrics, no internals. It is thus exactly the operational endpoint that F7 permits, and a usable touchstone for the open question Q3.

**The permission cut (F8).** The service runs as a separate, unprivileged user; the systemd unit sets `ProtectSystem=strict` and assigns the configuration and state directories separately. Writing to the configuration happens only in command mode.

**Configuration and access in one act (F9).** `config add-account` is the core: it configures the function for this application case — site, IMAP account, target folder — and creates the token for it, in one motion. The token embodies exactly this triple; callers never choose the account or the folder, neither of which is in their request. It is output exactly once (`imsvc_` plus 24 random characters); persisted are only its SHA-256 and a harmless short identifier (`tk_` plus six characters). `rotate-token` replaces it atomically.

**No administrative rights (F10).** There is no administration token and no administrative user in the service; the three roles of the specification are called operator, client/agent and user — an administrator does not occur. Every access is filtered on *(site, access)*, and foreign as well as nonexistent resources answer identically with 404.

**Outer edge (F11).** The service speaks HTTP exclusively and listens by default on `127.0.0.1:8025`; TLS is terminated by the proxy in front, which serves as the only published entry point. Container operation and a hardened systemd unit are provided for as the process form — the same artifact in both cases.

**State of play, honestly.** The specification is complete, the implementation by a development agent is running; a completion report was still outstanding when this version was written and will be added here. What is thereby evidenced is that this cut can be specified out completely and without contradiction for a real service — not how it holds up over years in operation. In particular the costs from §6 that accrue only with time — maintenance of the command code, ergonomics, the stiffness from F9 — have not yet materialized and are therefore not refuted either.

### Origin

The cut is not an invention of this dossier. The twelve-factor collection has long demanded that admin processes be run in an environment identical to the service's and that their code be shipped with the application code (Source 2). This dossier draws the sharper consequence from that — not "shipped along with" but "the same artifact in a different mode" — and connects it with two things that do not appear there: the one function (F1) and the creation of the access in the configuration step (F9), which together dissolve the permission topic before it arises.

## 9. Open questions

The first two questions are at the same time **assignments to the adopter**: their right answer depends on the deployment; whoever adopts the cut answers them for their own context and documents the answer.

**Q1** *(assignment)* Which permissions exactly does each mode need (F8) — which user, which directories, reading or writing? The cut is service-dependent; the concept demands only that it exist, that it be enforced in the process form and that it be documented.

**Q2** *(assignment)* How far does "one artifact" reach when the service consists of several components or runtimes? To be clarified is whether the principle holds per component — and who then runs the administration that is needed across component boundaries.

**Q3** Where does the boundary run between an operational endpoint and a management surface (F7)? A readiness point that says only "running" changes nothing and reveals nothing; a metrics output already reveals operational internals; a configuration display would clearly be administration. Is a rule needed for this, or does the principle "nothing that changes, and nothing that shows contents or internals" suffice?

**Q4** Do data migrations belong in a command (explicitly invoked) or in the start of the service mode (automatic)? The command path makes them a decision and fits F8; the start path makes them unforgettable and fits automated deployment procedures. Both are widespread, and the choice has consequences for rollbacks.

**Q5** How does one check that the check command and the start behavior really pass the same judgment (F5) — through shared code alone, or is a test needed that holds the two paths against each other?

**Q6** How does this cut relate to operating environments that bring an administration layer of their own with them (orchestrators with declarative configuration)? Does the command mode become superfluous there, does it shift — or does it remain the place for everything the platform does not know about?

**Q7** When does a service legitimately carry more than one function (F1)? The clear case is shared state that cannot be cut. Is a criterion needed for this — "two functions are one if neither is meaningful without the other", say — or does it remain a matter of judgment that one checks against the descriptive sentence?

## 10. Sources & related work

This is the only place in this dossier where references stand. Locations checked on 2026-08-15.

**Source 1 · Sister concept of this series: [capability-tokens-for-agents](../capability-tokens-for-agents/PROPOSAL.en.md).** — *What it stands for here:* The justification of the access pattern that F9 presupposes (one token, one capability, one place; server-side creation, one-time display, hash storage, individual revocation) and of the secret ranked order that F5 refers to; there too the self-registration that F9, with reasons given, does not adopt for this cut. Further depth, not a prerequisite for understanding.

**Source 2 · Adam Wiggins et al.: *The Twelve-Factor App*, factor XII "Admin processes".** https://12factor.net/admin-processes — *What it stands for here:* The ancestry of the principle: admin processes are to run "in an identical environment as the regular long-running processes of the app", and "admin code must ship with application code to avoid synchronization issues". F2 and F4 sharpen this into a common artifact; F1, F9 and F10 go beyond it. The collection additionally recommends configuration in environment variables — from which F5 deliberately deviates for everything except secrets, because a file is checkable and versionable as a whole.

**Source 3 · Draft-filing service of the author, repository `imap-service`.** https://github.com/schlingensiepen/imap-service — *What it stands for here:* The running example and the evidence in §8; all the values named there come from its short specification.

**Source 4 · Sister concept of this series: [self-describing-services](../self-describing-services/PROPOSAL.en.md).** — *What it stands for here:* The same figure of thought at the level of description — one source instead of distributed copies — where this dossier applies it to tooling and cut. Further depth, not a prerequisite for understanding.

**Placement.** This dossier and `self-describing-services` (Source 4) are deliberately separate and not two versions of the same idea: there stands a **design pattern for an interface** — how a service describes itself so that consumers can use it without installing anything. Here stands a **procedure** for getting such a service running quickly and without sprawl. They touch at exactly two points: the service built according to this procedure can adopt its description pattern from there, and the access that step 8 issues follows the pattern from Source 1. Each of the three dossiers can be applied without the other two.
