---
type: Proposal Scope
description: Security considerations, what the concept costs each role, where it applies, when this no longer holds, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

| Attack / risk | Typical course of events | Countermeasures |
|---|---|---|
| Takeover via the network surface | a vulnerability in the service mode is exploited | The process cannot write the configuration, hence cannot repurpose the function (F8); there is no administration function that could be reached (F7); and no access that would carry more than the one function (F10) |
| Privilege escalation via a captured access | a valid access artifact falls into the wrong hands | The access carries exactly the preconfigured variant of the function; target and scope are not in the request and cannot be shifted (F9) |
| Management surface as a way in | management path or debug route with a weak login | There is none: administration is a locally started command whose authentication is the server access (F7) |
| Version drift of the administration tool | a tool from an older version writes into the store | One artifact, one version (F2); commands run against the same delivery artifact (F4) |
| Administration from an external environment | a command from the developer's machine against the production store | Commands only in the same environment as the service (F4); write permissions only there (F8) |
| Secret in the artifact | configuration with credentials built into the image | State and secrets are outside (F6); the configuration file contains only references to secrets, not the secrets themselves (F5) |
| Existence oracle | systematic guessing at someone else's resources | Someone else's resources answer identically to nonexistent ones; the access limits the view to its own variant anyway (F9) |
| Outdated TLS library in the service | the service terminates TLS itself and is rarely updated | TLS is with the reverse proxy, which is built and maintained for it (F11) |
| Unintended exposure | the service listens on all interfaces | Local endpoint; the exposure is a deliberate proxy configuration (F11) |

Limit of the model: against an attacker who already has server access, nothing here protects any longer — the commands are then as open to them as to the operator, and that is intended: their authentication *is* the server access. If you cannot make this assumption, you will find below, under where it applies, the conditions under which this no longer holds.

## What it costs

This section enumerates what the design costs. Every point is pinned to the decision that causes it, and is listed here without a counter-calculation.

**The second function costs a second service (F1).** What suggests itself — "it can do that at the same time, surely" — is forbidden here, and the price is real: a second delivery artifact, a second delivery, a second store, a second entry in the proxy. If you have many small functions, you administer many small services; the saving is inside each individual one, not in their number.

**The artifact carries code the service never executes (F2).** The commands are in the same delivery artifact — they enlarge it, they extend the attack surface of the *image* (though not that of the running process), and they have to be maintained, tested and shipped along with it.

**The delivery chain wants maintaining (F4).** Build pipeline, registry, credentials of the target machine, and, for a non-public source, the question of who may pull the image at all: for an afternoon project this is the part that easily looks bigger than the service itself. If you forgo it and let the target machine build, you save this work — and pay with the multiplication of the artifact that F4 is meant to prevent.

**Command-line ergonomics have to be built (F2, F3).** An administration nobody likes to operate gets bypassed — with scripts that write directly into the store. So there is a need for expressive commands, usable outputs, reliable exit codes, a machine-readable form and a help text one understands without a manual. That is work which would be more visible in a web UI and is therefore often done more readily.

**Operational tasks need server access (F7).** Checking the configuration, issuing accesses, cleaning up operating data: for that there is no surface but the command line on the server. For the small service whose builder and operator are the same person, that is a saving and not a loss. If operations grow and an on-call or support team is to take on such tasks, they need system access — or a management surface is needed after all, with everything attached to it. The domain side is untouched by this: what the service offers as its function remains reachable over its network surface.

**Every change of the variant is a new access (F9).** If the same application is to write into a different folder in future, one field is not changed; instead an access is newly issued and the old one withdrawn — with everything that means at the caller's end. The stiffness is intended; it is not comfortable.

**The permission cut costs care in the deployment (F8).** Separate users, file permissions per directory, a command invocation under the right user — configuration work in the process form, error-prone and to be performed anew in every environment.

**The reverse proxy is an additional component (F11).** It has to be selected, configured, updated and taken into account in the fault case; fault-finding gains one more station.

**State outside means administering state (F6).** Named places, permissions, backups, restoration: what the artifact does not bring with it, operations must provide. The price of interchangeability is that there is something that is not exchanged.

## Where it applies

### Preconditions

**The task can be captured as one function.** There must be a sentence that says what the service does, without "and also" (F1). You recognize the situation in the attempt to write it.

**Anyone who administers the service has server access.** The commands forgo a login of their own because they presuppose it (F7). Meant are operational tasks on the service, not its domain use. You recognize the situation by who is to issue accesses and change the configuration — if those are people other than those responsible for the system, the assumption no longer holds (below).

**The process form allows one-off invocations in the same environment.** Whether systemd, container runtime or a plain binary: it must be possible to execute the same delivery artifact briefly and locally (F4).

**State can be stored outside.** Named directories, a volume, external storage — anything that survives the change of version (F6).

### When this no longer holds

**The service must carry several independent functions.** If two things necessarily share the same state and cannot be cut into two services, the permission distinction returns: then accesses of different kinds are needed, a rule as to who gets which — and with it the very administration that F9 and F10 abolish. You recognize the situation by the fact that one variant of the access is to permit one method and not the other.

**Operational tasks are to be performed without system access.** Meant are tasks on the service itself — issuing accesses, changing the configuration, cleaning up operating data — that people without server access are to take on: on-call, support, domain administration. This no longer holds because F7 draws its simplicity precisely from the server access being the authentication; otherwise one would have to give many people system rights in order to give them operational tasks. Then an authenticated management surface is needed after all — and with it the login, permission and evidence questions that F7 avoids. You recognize the situation by the question "Can somebody do that without server access?", and by the fact that it is asked regularly. This does not cover the domain use: that users use the function over the network is the purpose and not a reason this no longer holds.

**The platform does not allow a one-off invocation.** Some operating environments permit only the rollout of long-running processes, no access to a running instance. Then F4 cannot be redeemed; the commands would have to be rolled out as a separate, short-lived process of the same image — feasible, but the design becomes cumbersome, and one should know that beforehand.

**The service consists of several runtimes.** If parts are built in different languages, there is no longer "one binary". Then the principle holds only per component, and the question of the common administration path arises anew; the agent guide makes recording the answer a duty of adoption.

**Accountability per person.** Where it must be proven who performed which operational action and when, "had server access" is not enough; the process form has to provide it, or the management surface returns.

### What it does not solve

**Scaling and failure tolerance.** Whether a service runs in several instances, how loads are distributed and failovers arranged, this concept does not say; it only makes the artifact interchangeable enough for such procedures to remain possible.

**Observability and backup.** Logs, metrics, alerts, data backup and restoration remain operational work outside this design.

**The domain user and role model.** Users, roles and grants *within* the function of a service are orthogonal to this design: it neither forbids them, nor prescribes them, nor replaces them. Accesses are here the one defined way in from outside; how a service models things internally remains its own business. This dossier says only that the *administration of the service* needs no such model (F10).

### Not yet settled

* Where the boundary between an operational point and a management surface runs (F7): a readiness point that says nothing but "running" changes nothing and reveals nothing; a metrics output already reveals internals; a configuration display would be management. Until further notice the principle holds: nothing that changes, and nothing that shows contents or internals.
* How one verifies that the check command and the start really pass the same judgment (F5): through shared code alone, or with a test that holds both paths against each other.
* How this design relates to operating environments that bring their own management layer (orchestrators with declarative configuration): whether the command mode becomes superfluous there, shifts, or remains the place for everything the platform does not know.
* When a service legitimately carries more than one function (F1): the clear case is shared state that cannot be cut; whether that needs a criterion or the test against the one-sentence description suffices.
