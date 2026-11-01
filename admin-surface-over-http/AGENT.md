---
type: Proposal Agent Guide
name: admin-surface-over-http
description: Self-contained guide for an agent that builds, reviews or operates a second administration surface over HTTP beside the command line of a service that must run where the operator has no shell; procedure with the choices to decide, rules per role, acceptance checklist, common mistakes, self-check.
---

# Agent guide

## Purpose

Apply this guide when a service built as one artifact with command-line administration must be run where the operator has no shell and no one-off invocation against the running container (a container console, a shipped deployment file, a cluster with management gestures only), and you build, review or operate a second, complete administration surface over HTTP beside the command; or when, as an agent, you administer such a service for its operator through that surface.

Three roles use this guide; apply the rule set for yours.

- **Builder**: you build or review a service that follows the cut "one artifact, several modes, administration is a command", and it has to run where its operator cannot execute a command. You are deciding whether to add an administration surface over HTTP, building it, or reviewing one.
- **Operator**: you run such a service through a container console or a shipped deployment file, and you must take it into operation, issue and withdraw accesses, and replace the administration secret without a shell.
- **Administering agent**: you act for an operator and administer the service through its administration surface. Your rules come from that surface's own guide; the short set below is what holds regardless.

This guide is complete on its own, except for two things: the general handling of start secrets (ranking, report of lower ranks, replacing in two moves, the once-only exception) comes from the imported concept `where-a-secret-comes-from`, and the rule that every operation exists exactly once with every surface a pass-through onto it, held by a structural test, comes from the imported concept `one-operation-many-surfaces`. The rules below write out what both mean for this surface, and the choice `where-a-secret-comes-from` leaves open is decided in the procedure. Ask the user for those bundles if you need more. Rules are named in parentheses as provenance.

The concrete forms below (`/admin/v1`, `<SERVICE>_ADMIN_TOKEN`, the value `new`, the 32-character floor, a second port) are patterns, not a house convention: the words and numbers are interchangeable, the shape is not: own port, own name, one secret in a fixed ranking of sources, shown once in the log.

## Procedure

**1 · Confirm the need.** Name the concrete environment in which the operator cannot run a command against the service. If a shell or a one-off invocation exists after all, stop: the surface would cost its price for nothing. (F1)

**2 · Decide the choices with the user, never on your own.** Adopting this extension decides the base concept's choice on the channel of the commands: commands also come in over another channel, here HTTP. Record that, and record how the permission cut is established in the channel (two processes, or the waiver in the security considerations; step 9). For the administration secret this extension decides two choices of `where-a-secret-comes-from`: when no secret is set, generate it and show it exactly once; when it is set but unusable, abort the start. The remaining choice there, further sources (a platform's secret manager as a rank of its own or as a feeder to the variable or the file), is decided with the user. Group settings where the same answer fits, present each group with the options and their consequences, and record the decision per group. *Result:* the recorded decisions.

**3 · Secure the operations per `one-operation-many-surfaces`.** Make sure every administrative action exists as a callable function apart from any command and any handler, raising its own error kinds. If the logic lives in the commands today, extract it first. (F2)

**4 · Write the structural tests of `one-operation-many-surfaces` before the first handler,** over both pass-throughs. Whitelist of imports, list of operation names, file system ban with named exceptions, "every command and handler calls an operation". Run each once with its protection removed. (F3)

**5 · Build the administration as a separate application** on its own port and prefix, with its own check of the secret that compares in constant time and refuses "no value", "wrong scheme" and "wrong value" identically. Add the second host name to the proxy example with the tightening comment. (F4)

**6 · Wire the address** as an operational parameter that is empty by default; set it in the container image's environment; leave the host package alone. (F5)

**7 · Resolve the secret** in the fixed ranking with the single log line at generation, the `new` sentinel, and the start aborts for short, unwritable, unreadable and unbindable. (F6, F7)

**8 · Add the lower-rank report at start** and the two-move sentence to the entry page and the deployment file. (F8)

**9 · Write the security considerations:** root not leaf, revocation, change, lifetime, loss, the residual window; and, until two processes exist, the waiver sentence. (F9, F12)

**10 · Build the routes as pass-throughs:** create with access and base URL once, list without secrets, remove, set password, rotate, and the queue and pool actions; no update route, and a test that asserts its absence. Mirror every command, store-wide actions included. (F1, F2, F10)

**11 · Write the entry page** with its four statements, the refusals in this surface's words, the format statement for every valid secret, and the readiness point; run every check the other surface has on this one too. (F11)

**12 · Build the image smoke test:** both ports answer, the administration description is readable without a secret, the secret is logged once, accepted, one not issued is refused, a restart is survived, no second log line. (F6)

**13 · Let an operator who did not build the service** take an instance into operation from the log line and the entry page alone, and treat every question he has to ask as a defect of the entry page or a refusal. (F11)

**14 · Plan the two-process form as the next step** and write down what blocks it. (F12)

## Rules

### Builder

MUST:

1. Build the administration surface only for operators who have neither a shell on the host nor a one-off invocation against the running container, and if you build it, make it **complete**: every administrative action the command line can do, the surface can do too, because an operator without a shell cannot fall back to a command for the rest. That includes actions that span **all** items of the store, not only those per item: a loop the caller runs over the account list is a workaround, not a counterpart. Keep the command line; it stays the normal way wherever a shell exists. Name in the service's documentation that the surface departs from the cut the service is built on (no administration surface, no administrative rights), under which condition it is needed, and what it costs. (F1)
2. Under the imported `one-operation-many-surfaces`, implement every administrative action **exactly once**, as a callable function that knows neither HTTP nor process exit: it takes arguments, acts, and returns a result or an error kind of its own vocabulary. Make the command line and the HTTP surface **pass-throughs** onto those functions: each command and each handler reads its arguments, calls one operation, and returns what came back. Give each surface exactly one translation function (error kinds to HTTP statuses on one side, to exit codes on the other) and let both emit the same response models with the same field names. No surface writes the store, touches the file system or talks to the target system; the operations do. A surface that needs something no operation offers gets a new operation, not a search of its own. Neither surface calls the other: the command never needs the administration secret or a running service. (F2)
3. Hold that property with the **structural test** of `one-operation-many-surfaces` in the build that parses the source of both pass-through packages and fails where an operation is performed instead of called. Check at least: that a pass-through imports from the service only the operations and the operational parameters (a **whitelist**, never a blacklist); that the names of the operations themselves (read or write the store, generate or verify a token, enqueue, claim, revive or discard a job, store, list or delete a file, the client for the target system) appear in no pass-through; that no pass-through opens, writes, lists or removes anything on the file system, with any exception named in the test by function and file rather than matched by pattern; and that every command and every handler calls at least one operation, and everything it calls really is one. Run each of these assertions once with its protection removed and see it fail before you trust it. (F3)
4. Make the administration a **separate application on its own port** with its own path prefix (pattern `/admin/v1`) and its own self-description; publish it through the proxy under its **own host name**, ideally in another zone, and tighten that name as far as the environment allows. Leave the user-facing surface untouched: no administration path, no debug path, a readiness point that says only "running"; figures only behind the administration secret on the other port. Share no credential between the two surfaces, structurally: the administration secret is not in the store, an access is never compared against it, so each is refused on the other side without a rule anybody has to remember. Never mention the administration in the user-facing self-description. Abort the start if the administration port cannot be bound. (F4)
5. Ship the surface **switched off**: an empty operational parameter for its address means no port, no secret, and no secret generation, even when the environment variable for the secret is set. Let the delivery form whose addressee is the operator without a shell switch it on: the container image sets the address in its environment (as environment, not as an entry-point argument, so a deployment file can override it and switch the surface off again); the host package sets nothing. Say in the head of every deployment file that switches it on that it does so and what that costs. (F5)
6. Take the administration secret, when the surface is on, in the ranking every start secret follows: **(1)** an environment variable (pattern `<SERVICE>_ADMIN_TOKEN`); **(2)** otherwise a file the service stored beside its store on first start, with tight permissions; **(3)** otherwise **generate one** server-side, high-entropy, with a recognizable prefix, store it there, and write it into the log **exactly once**, with the sentence that this is the only time it is shown. That single line is the one deliberate exception to "no secret ever reaches a log", and it is cut narrowly: exactly one value, exactly once, only at generation; never for a secret the operator supplied, never on restart, never for an access. Let an agreed value in the variable (pattern `new`) force regeneration. In the build, start the shipped image before publishing it and require that the secret appears in the log exactly once, is accepted, that one not issued is refused, that the secret survives a restart, and that it is not logged a second time. (F6)
7. Never fall back silently. A secret that is set but unusable (pattern: shorter than 32 characters, a coarse floor that rejects typed words, not a measure of entropy) aborts the start with a message that says what to do. So does a secret file that cannot be written or cannot be read, and an administration port that cannot be bound. The service never generates a secret of its own in place of an unusable one and never runs without the administration when it was asked for. (F7)
8. Because a ranking of sources makes replacing easy and deleting invisible: at start, when a higher rank is in force and a lower one is **also set** (the variable wins, but the file from first start is still there), log a warning that names where the lower rank lies and how to clear it. Give the operator a way to clear it without a shell (setting the agreed value and restarting replaces the file). State in the surface's entry page and in the deployment file that **changing** the administration secret is two moves (replace the upper rank **and** clear the lower one), not one. Never let a set upper rank silently hide a lower one. (F8)
9. Treat the administration secret as the **root** of the rights hierarchy, not as a capability token: it cannot perform the function itself, but it can issue and withdraw every access that can. Provide, and document: **revocation** (the secret is replaceable at any time without precondition: remove the variable or set it to the agreed value, restart, take the new value from the log; and withdrawing any issued access is never harder than issuing it); **change** (two moves, upper and lower rank); **lifetime** (it does not expire by itself, it lives in the process definition or its file as long as the operator leaves it there, and the service shows the age of issued accesses, last use per account, instead of expiring them); **loss** (there is no re-display; a lost secret is replaced, not looked up). Never let it travel in an address, on a surface for weak callers, or in a transcript; treat it like an administrative password. Name the residual risk in the security considerations: between compromise and detection, an access can be issued that works until it is withdrawn. (F9)
10. Let secrets flow **in only**: an issued access appears once, in the answer that created or rotated it, together with the base URL it is valid for (the user-facing surface runs on another port under another name and cannot be inferred from the administration's description), and never again; listings show identity, name, target and last use, never a secret and never a stored third-system password. Offer **no general update** of a variant: create, remove, set the third-system password, rotate the access; anyone who wants to change target, address or procedure removes and re-creates and receives a new access. (F10)
11. Give the port what an operator without a shell needs when he has nothing else (no manual page, no command, no `--help`): **(1)** an entry page at its base URL, readable without a secret and containing none, that says what this port administers, that it is **not** the user-facing surface, what one needs (the administration secret and where it comes from: variable, file, log) and what this port cannot do (perform the function, show secrets, change existing entries); **(2)** refusals that name a way **this** surface has, never a command he cannot run, never a header or a call of the user-facing surface; **(3)** wherever the secret's format is stated, a statement that describes **every valid** secret and not only a generated one, and wherever an example is shown, a visible placeholder; **(4)** a readiness point that answers without a secret and says only "running". Ship **no anchor package** here. And whatever checks you run for one surface, run them for the second, never a hand-made copy. This is all this guide orders for the description of the surface; a service that also describes itself under the neighboring concept of self-describing services owes this surface everything that concept demands, without discount, but that follows from there, not from here. (F11)
12. Re-draw the permission cut the base cut loses: run the user-facing surface and the administration surface in **two processes with two users**, and let only the administration process write the store, including storing the generated secret on first start. Until that is built, state in the service's security considerations, as a sentence and not a footnote, that a takeover of the user-facing surface can write the store and thereby issue accesses, and that this is accepted. A waiver that is written nowhere is not a waiver. (F12)

NEVER:

- Never switch the administration surface on where the operator has a shell, and never offer it as a convenience. (F1)
- Never give one surface a capability the other lacks, and never implement an operation in a surface instead of calling it. (F1, F2)
- Never rely on behavioral tests alone to keep the two surfaces on one operation; two correct copies pass every behavioral test. (F3)
- Never put administration routes under a path prefix of the user-facing surface, and never accept an access on the administration or the administration secret on the user-facing surface. (F4)
- Never generate or store an administration secret when the surface is switched off. (F5)
- Never log the administration secret more than once, never log a supplied one, and never log an access. (F6)
- Never fall back to a generated secret when a set one is unusable, and never run without the administration when it was configured. (F7)
- Never let a set upper rank hide a lower one without saying so. (F8)
- Never put the administration secret into an address, a surface for weak callers, or a transcript; never build a "show the secret again" function. (F9)
- Never return an existing secret or a stored third-system password, and never offer a general update that could redirect a stored credential. (F10)
- Never demand a full self-description of the administration surface on the strength of this guide; the entry page, the refusals, the format statement and the readiness point are what it orders. (F11)
- Never keep a check for one surface as a hand-made copy for the other; run the same check on both. (F11)
- Never leave the one-process, same-rights waiver unstated. (F12)

### Operator

MUST:

1. Take the service into operation without inventing a secret: start it with the variable unset, read the one warning line from the log, store the value in your platform's secret store, and expect no second line; there is none. (F6)
2. If your platform can inject secrets into the container's environment, set the variable from there before the first start; the service then generates nothing and logs nothing. (F6)
3. If your logs go into a shared collector, prefer the variable, or replace the generated secret after collecting it. (F6)
4. When you change the secret, make two moves: replace the variable **and** clear the file from first start (set the variable to the agreed value once, restart, take the fresh value from the log, then set your intended value). One move leaves a valid secret nobody knows lying in the volume. (F8)
5. Publish the administration under its own host name, tightened to your network, and never map its port to the open internet; where you run without TLS, know that the secret, every issued access and every third-system password travel in the clear. (F4)
6. Treat the secret as an administrative password: never paste it into an address, a chat, a ticket or a surface for weak callers. If it leaks, replace it; that is one restart. (F9)
7. Read the start aborts: a value shorter than the floor, an unwritable volume, a busy port stop the service with a message in the log, and they do not fall back. (F7)

NEVER:

- Never switch the surface on where you have a shell. (F1)
- Never expect the service to show you the secret again. (F6, F9)
- Never assume a changed variable retired the old secret. (F8)

### Administering agent

MUST:

1. Read the administration entry page and guide first; they are the only description you have of this surface, and they say that this port is not the one that performs the function. (F11)
2. Confirm every acting operation with the person it affects before you run it: creating an account hands out a credential into their mailbox, removing with purge deletes data for good, rotating stops their client at once. (F11)
3. Hand an issued access on through a channel other than this conversation, together with the base URL the answer named for it, and never repeat it. (F10)
4. Keep the administration secret out of everything except the request header: never quote it, never write it into a file that is not your secret store, never use it against another base URL than the one it belongs to. (F9)

NEVER:

- Never look for a way to change an existing account; remove and re-create, and tell the operator a new access results. (F10)
- Never try to read an existing access or password back; there is no such call. (F10)

## Acceptance checklist

- [ ] Is the surface built only for an environment without shell and without one-off invocation, is it complete relative to the command line (store-wide listings and actions included, not only the per-account forms), does the command line still exist, and does the documentation name the departure and its condition? (F1)
- [ ] Does every administrative action exist exactly once as a function without HTTP or exit knowledge, do both surfaces call it, do both have exactly one translation function, and do both emit the same field names? (F2)
- [ ] Does a structural test over both pass-through packages fail on a forbidden import, an operation name, a file system call, or a command or handler that calls no operation, and has each assertion been seen red once? (F3)
- [ ] Own port, own application, own prefix, own host name; is an access refused on the administration and the secret on the user-facing surface without a rule; is the administration absent from the user-facing description; does a busy port abort the start? (F4)
- [ ] With the address empty: no port, no secret, no generation even with the variable set? Does the image set the address as environment, and can a deployment file switch it off? (F5)
- [ ] Variable, then file, then generation; exactly one log line, only at generation; `new` regenerates; does the image smoke test count exactly one occurrence, accept it, refuse one not issued, survive a restart without a second line? (F6)
- [ ] Do a short secret, an unwritable or unreadable file and an unbindable port each abort the start with a message, with no silent generation? (F7)
- [ ] Does the service warn at start when the variable is in force and the file is set, naming its place and how to clear it; do entry page and deployment file say a change is two moves? (F8)
- [ ] Are revocation, change, lifetime and loss documented; is the secret replaceable without a shell; is the residual window named? (F9)
- [ ] Do listings show no secret; is there no update route (asserted); does the create answer carry the access once together with its base URL? (F10)
- [ ] Does the entry page say what this port administers, that it is not the user-facing surface, what one needs and what it cannot do; does no refusal recommend a command or a user-facing header; does the secret's format statement accept every valid secret; does readiness answer without a secret; is there no anchor package; and does every check of the other surface run on this one rather than a copy? (F11)
- [ ] Either two processes with two users and only the administration process writing the store, or the waiver as a sentence in the security considerations? (F12)
- [ ] Are the decisions recorded: the channel of the commands with how the permission cut is established, and the further sources for the administration secret?

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Administration surface on "because it is convenient" | Only where no shell and no one-off invocation exist; otherwise leave it off (F1) |
| Two commands without an HTTP counterpart, description says "complete" | Mirror every command; the operator without a shell has no fallback (F1) |
| Store-wide command (list everything, retry every failed job) exists over HTTP only per account | Add the store-wide route; a loop over the account list is a workaround the operator without a shell has to build himself (F1) |
| A helper in a command "just for output" that reads the store | Make it an operation; the structural test must go red on the import (F2, F3) |
| Structural test written as a blacklist | Whitelist of allowed imports; a blacklist passes every module nobody thought of (F3) |
| File system exception matched by pattern | Name it by file and function; a narrower pattern admits the next reader (F3) |
| Administration under `/admin` on the user-facing port | Separate application, separate port, separate host name (F4) |
| One bearer check with a role flag | Two credentials that structurally fail on the other side (F4) |
| Address set as entry-point argument in the image | Set as environment so a deployment file can override it (F5) |
| Secret generated even when the surface is off | No address means no port, no secret, no generation (F5) |
| Secret logged on every start "so it can be found" | Exactly once, at generation; a supplied one never (F6) |
| Short secret accepted, or replaced by a generated one silently | Abort the start with a message (F7) |
| Variable changed, file from first start left in the volume | Warn at start about the lower rank also set; a change is two moves (F8) |
| Secret treated as an API key and pasted into a GET-only surface | It is the root; never in an address, never on a weak-caller surface (F9) |
| "Show token" endpoint for lost secrets | Rotate; there is nothing to look up (F9, F10) |
| `PATCH /accounts/{label}` with a host field | No update; remove and re-create, new access (F10) |
| Token returned without the base URL it is valid for | Return the user-facing base URL with the token, with the proxy caveat (F10) |
| Full self-description demanded of the administration surface on the strength of this guide | This guide orders the entry page, the refusals, the format statement and readiness; anything beyond comes from the neighboring concept, if the service applies it (F11) |
| Description check for the administration copied by hand | Run the same check on both surfaces (F11) |
| Format statement says the secret has the generated prefix | Describe every valid secret; a supplied one has no prefix (F11) |
| Waiver of the permission cut nowhere written | A sentence in the security considerations, or two processes (F12) |

## Self-check

Before you report completion: can you name the environment in which no shell and no one-off invocation exist, the one operation behind each administrative action, the structural test that holds it and the assertion you saw red, the port and host name of the administration, where the administration secret comes from and how it is changed in two moves, the four answers of the root (revocation, change, lifetime, loss), and the recorded decisions, including how the permission cut is established? If any of these is missing, you are not done.
