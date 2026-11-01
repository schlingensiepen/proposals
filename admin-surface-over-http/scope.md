---
type: Proposal Scope
description: Security considerations of a second administration surface with a root secret, what it costs each role, where it applies, when the answer flips, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

This surface reintroduces three things the base cut had abolished: an administration surface on the network, a secret with administrative power, and a service process that writes the store. The security considerations begin with that admission; the table checks whether the limits hold that are meant to make it bearable. Where a row goes beyond the built implementation, it says so.

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Administration surface published by accident | a proxy rule is missing or wrongly ordered; a path prefix slips into the user-facing surface | own port, own application, own name: what is not routed is not reachable (F4); off as shipped (F5); deployment file without a port mapping for the administration |
| Administration surface as a way in around the clock | a vulnerability in the administration application is exploited from outside | switched on only where needed (F1, F5); kept tight at the proxy: source network, access network, second factor (F4, F9); the same check rule for both surfaces instead of a copy (F11); where the service describes itself, that concept also covers this surface, external review included (see the concept) |
| Secret in the log is harvested | logs run into a collector with a wider readership, or are retained | exactly one value, exactly once, only at generation, never for a set one, never at restart (F6); the smoke test in the build counts the occurrences (F6); where logs are collected, the operator hands the secret in through the variable and does not trigger the exception at all (F6), or replaces the generated one after collecting it (operating advice, beyond the implementation; not yet settled, below) |
| Silent fallback to another secret | set value unusable, file not writable, port taken; the service runs anyway, under conditions other than those ordered | aborted start with a message in each of these cases (F7) |
| Latent access from a lower rank | variable replaced, file from first start stays; the variable is missing once later, and a secret applies that nobody knows | report of lower ranks also set at start, with place and how to clear them; change described as two moves (F8). **Not built in the implementation; the incident happened there** (see the evidence) |
| Root on the network: captured administration secret issues accesses | between compromise and detection an access is issued that works until it is withdrawn | honestly named residual gap (F9): every issued access narrow and individually revocable, noticeable to the legitimate holder, root replaced with one restart; a time-limited issuing window is not buildable here (see when the answer flips) |
| Administration access becomes a key ring | a listing gives out existing accesses or stored third-system passwords | output only at creation; listings show identity, never secrets (F10) |
| Redirection of stored credentials | the target address of an existing entry is changed to a foreign server; the service delivers the password there at the next operation | the function does not exist: no general update, only remove and create anew with a new access (F10) |
| Guessing the administration secret | the address is reachable, the secret weak or the comparison reveals something | generated secrets high-entropy, set ones with a minimum length (F6, F7); comparison in constant time; the same refusal for "no value", "wrong scheme" and "wrong value" (F4); throttling is the operator's business at the proxy. **Not built in the service** |
| Wrong login on the wrong surface | an access to the function is presented on the administration surface or vice versa | structurally separate login: the administration secret is not in the store, an access is never compared with it (F4); the entry page says this port is not the user-facing surface (F11) |
| Takeover of the user-facing surface writes the store | a vulnerability in the user-facing surface (uploads, foreign input); the process has the rights of the administration surface and issues itself accesses | two processes, two users (F12). **Not built in the implementation**; there the waiver stands as a sentence in the security considerations, as F12 demands as the minimum |
| Outbound surface to foreign hosts | probing a target system connects to an address the caller chooses | reachable only with the administration secret; opens nothing a holder of this secret could not reach anyway through a created account; the answer contains no credentials (F10) |
| Two writers on the store | command and administration surface write at the same time; a change is lost | version stamp on read, expected on write, non-blocking lock in a file of its own: properties of the one operation, not of a surface (F2); visible over HTTP as `ETag` and `If-Match` |
| Administration on the open map | the self-description of the user-facing surface names an administration route; a stranger learns where the root lives | the administration does not appear there, by construction, because the document is built from another application (F4, F11) |
| Reviewer with the root secret | an agent without inside view gets the administration secret for an external review and really acts | an instance of its own for the review, whose root is replaced afterwards; never the secret of the productive instance (F9; the external review itself is the business of the neighboring concept, see the concept) |

Limit of the model: against an attacker who has the host system or reads its logs, nothing here protects; whoever can read logs could also swap the application, and the service is exactly as open as its host. Against an operator who puts the administration surface on the open network without need, or switches it on where he would have a shell, no cut protects. And against the period between compromise of the root and its detection, this concept offers limitation, not prevention.

## What it costs

This section enumerates what the concept costs. Every item is pinned to the rule that causes it, and stands here without a counter-calculation.

**A second surface with everything a surface needs (F1, F11).** Own application, own entry page, own refusals in its own words, own readiness point, own checks that must be the same as those of the other surface; and whoever lets the service describe itself has to write and check a second complete description here. Every administrative action then wants explaining twice: as command help and at the port, with the same claim.

**The core makes the surface usable for an operator, not for a foreign agent (F11).** What this proposal demands is enough for a human with the entry page and the patience for refusals. An agent told "set up an account for me there" finds on a surface that has only the core no contract, no guide, no examples; it guesses, or it needs the source code. Whoever wants to leave administration to agents needs more for it than this proposal lays down, and the more comes from another concept (see the concept), with that concept's whole price.

**Every operation has two openings that need maintaining (F2).** The operation exists once, but a pass-through has one opening per operation, and both want updating when an operation gets a field. The structural test prevents a second implementation arising there; it does not prevent an opening being forgotten. The author's implementation had two commands without an HTTP counterpart and noticed it by a claim that was not true; and to this day it has three store-wide actions that exist over HTTP only per account, so the operator without a shell has to assemble them from the account list. An opening that spans everything on one side and only one item on the other strikes no test and strikes the operator only at the twentieth account.

**The structural test is code, and it ages (F3).** A whitelist of permitted imports, a list of operation names, a named exception: every new function, every new operation, every renaming touches it. And with every change it wants the red check: remove the protection, see red, restore. Whoever skips that has a list of names in which the one name is missing.

**Two names in the proxy, one more port in the container (F4).** The administration needs its own host name, its own certificate, its own block in the proxy configuration, and maintenance of the restriction that is to stand there. Every change to what goes over the wire has to be followed up at two names.

**Off as shipped means two delivery forms behave differently (F5).** The image switches on, the package does not. Whoever ships both explains in both heads what applies, and an operator who moves from one to the other finds the administration on or off without having changed anything.

**The log becomes a secret channel (F6).** One value, once; but it stands there, and logs are collected, retained, passed on. The exception needs an operating note that would be superfluous elsewhere, and the operator whose logs run into a collector must know that he should set the variable instead of triggering the exception, or replace the secret after collecting it.

**Aborted starts instead of fallbacks cost rounds (F7).** A secret that is too short, a store that is not writable, a port that is taken: the service does not start, and the operator without a shell sees that only in the log. Each of these messages must say what to do, and the operator must find it; in the as-shipped state of a container console that is one more click nobody makes before wondering.

**The ranking costs the report of lower ranks (F8).** An output that nobody reads in the normal case and that still wants building and maintaining; the author's implementation does not have it, and the incident it is meant to prevent happened there. And changing the secret is two moves instead of one, for good.

**A root that did not exist before (F9).** The most expensive artifact of the service is now reachable over the network, stands in a deployment file and does not expire. It wants treating like an administrative password (storage, handing on, replacement, evidence), and that discipline is operational work the base cut did not know. Doing without the time-limited window is honestly named and remains a waiver: between compromise and detection, issuing is possible.

**Deleting from afar (F1, F10).** Removing an account together with its data, emptying the queue, cleaning up the pool: operations that used to presuppose server access are now one authenticated call away. The service does not delete more than before, but the reach of who can trigger it has grown.

**Changes cost new accesses, now also where they would be convenient (F10).** Whoever administers over the network expects an update; there is none. Every correction to a variant is removing and creating anew with a new access, and the consumer switches over.

**The permission cut falls or costs two processes (F12).** In the one-process build the user-facing surface writes the store; that is a sentence in the security considerations and a risk one carries. The two-process build costs a second process definition, two users, file permissions per directory, a second rollout, and the question of who stores the secret at first start; work the author has not yet done.

## Where it applies

### Preconditions

**An operator without a shell, or a platform without a one-off invocation.** The surface is an answer to this situation and to no other (F1). You recognize it by the fact that somebody can start the service but nobody can run a command against it: container console, shipped deployment file, cluster with management gestures. Where a shell is a matter of course, the base cut applies, and this surface stays off.

**A base cut whose operations exist as callable functions.** The pass-throughs presuppose that there is something to pass through to (F2): administrative actions that know neither HTTP nor process exit. A service whose administration logic sits in its commands must first extract it before it may build a second surface. You recognize the situation by a command that does more than read arguments, call a function and print.

**A log the operator can read, and as few others as possible.** The log is the only back channel (F6). The operator must reach it through his platform, and whoever reaches it besides him has the root. You recognize the situation by the question who sees the container logs and how long they are retained.

**A proxy that can carry a second name and keep it tight.** F4 demands an own name and the possibility of keeping it tighter than the user-facing surface. Without a proxy (in a trusted network, without TLS) the own port remains, and the administration address then belongs all the less on the open network; the deployment file says so in its head.

**A secured connection to the administration surface.** Over it travel the administration secret, every issued access and every third-system password on its way into the store. Without TLS all three stand in plain text on the wire. You recognize the situation by an administration base URL without `https`.

### When the answer flips

**There is a shell after all.** Meant is an environment in which the operator can run a command against the service: on the host, by one-off invocation in the container, through a task mechanism of the platform. The answer flips because F1 provides the surface only for the case that it cannot be done otherwise; whoever switches it on anyway carries root, network surface and loss of the permission cut without need. You recognize the situation by an operator administering over the network although the command would work.

**The platform has a secret management.** Meant is an environment that hands secrets into the container's environment without a human touching them. The answer does not flip entirely, but it shifts: the top rank of the ranking becomes the only one, generation and the log line disappear, the lower-rank duties of F8 have no object because there is no lower rank; and whoever still lets the service generate there triggers the exception to the rule against secrets in logs without needing it. You recognize the situation by the platform knowing "secret" as an object of its own.

**The logs run into a collector with a wide readership.** Meant is an operation in which container logs are collected automatically, retained for a long time and read by more people than the operator. The answer flips because the exception from F6 is then no longer a narrow one: the one value stands where many read, for as long as it is retained. The way out is the top rank (hand the secret in instead of out) or replacing it after collecting it (not yet settled, below). You recognize the situation by nobody being able to say who reads the logs.

**The delivery has a channel inward.** Meant is an environment in which the one who delivers can place files on the host system before the service starts: a script over a connection, a provision by a human with access. The answer flips because then the first secret can be put in and a time-limited issuing window becomes buildable that opens the root only during delivery; that is the better answer than F9, and this surface explicitly takes its place only where the environment does not allow it. You recognize the situation by a rollout beginning with a copy operation.

**Administration is to be done by several persons with their own identity and a duty of evidence.** Meant is an operation in which it must be proven who issued or withdrew which access when, or in which different people are to be allowed different amounts. The answer flips because a root is a secret and not a person: F9 gives everyone who has it everything, and no one a name. Then a rights and evidence model is needed that this proposal explicitly does not build. You recognize the situation by the question "who issued that?", to which the answer "somebody with the administration secret" does not suffice.

**The service carries several tenants in one instance.** Meant is a store holding variants of different owners that are not to see each other. The answer flips because one root per instance reaches all variants of the instance; the base cut solves tenants as separate instances, and this surface takes that over. You recognize the situation by two operators being meant to administer different parts of one store.

### What it does not solve

**Evidence per person, and roles.** Who did what when, and who may do what: the surface knows a secret and no identity. What the base cut excludes from its scope, this proposal excludes as well.

**Expiry and throttling.** The administration secret does not expire (F9), and the service does not throttle failed attempts on the administration surface; both are the operator's business at the proxy or in his calendar. Whether the root needs a calendar precisely for that reason is not yet settled (below).

**Self-registration.** A way in which a consumer obtains an access himself, which is then approved, is the consumer side of onboarding; this surface is the operator side. Whether the administration surface would be the approval path for such a request is not yet settled (below).

**Delivery without a channel inward as a whole.** This proposal answers the one question of how the first secret comes into being. How a pulled service is otherwise rolled out, checked and taken back without anybody writing into it from outside is a subject of its own.

**The split into two processes.** F12 demands it; this proposal does not show it, because the author has not built it. What the process form looks like, who stores the secret at first start, and whether the interfaces really stay unchanged is not yet settled (below).

**Rights management within the function.** What a service knows in its domain about users, roles and grants runs across this proposal; here it is solely about administering the service.

### Not yet settled

* **Whether the description of the administration surface is readable without a secret.** F11 demands an entry page readable without a secret; whether a fuller self-description should be open as well is not settled. The argument of the user-facing surface is weaker here, because the audience already knows the administration exists, and stronger, because the operator without a shell needs the description before he has fetched the secret from the log. The author's implementation is open and coupled to the switch of the user-facing self-description; whether the administration needs a switch of its own is not answered.
* **Whether issuing accesses needs a second secret** that stands only in the log, separate from the one in the process definition, or a time-limited window opened by a restart. Either would harden F9 without requiring a channel inward; either costs a move or a restart at every setup, and F9 gives the reason why that would be switched off or left open.
* **How this surface relates to self-registration,** in which a consumer obtains an access without rights himself that somebody approves on the server side: is the administration surface the approval path for such a request, and would that be a merger or a duplication?
* **Whether the root needs a calendar.** The administration secret does not expire because it may stand in a deployment file and an expiry would lock out the operator without a shell. Whether precisely the root needs a calendar a leaf does not, and how a service without a shell and without a channel outward reports that its root is old, other than through the same log, is open.
* **What happens after the generated secret has been collected,** where logs run into a collector: mark it as "collected" and warn at the next start, invalidate it itself after a deadline, or an operating note in the description rather than in the mechanism.
* **What the two-process form looks like.** F12 demands two processes with two users and claims the interfaces stay unchanged. Who stores the administration secret at first start, if the user-facing process may not write the store and the administration process may not be running yet? And what does the process form look like on a container console: two containers from one image with one volume and different users, or something else?
* **How far the structural test of F3 goes.** The author's implementation checks imports, names, file system access and that every command calls an operation; it does not check that every handler calls **exactly one** operation and does nothing between call and return. Whether the sharper form is maintainable, or the whitelist is already the limit of what can be said about structure and the rest belongs in the behavioral test over both surfaces, is open.
