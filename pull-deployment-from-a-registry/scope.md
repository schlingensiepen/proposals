---
type: Proposal Scope
description: Security considerations of a pull deployment, what the concept costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

| Danger | Typical course of events | Countermeasure |
|---|---|---|
| **A slipped-in state** | `latest` is written from a working branch; an operator who pulls without a name runs an experiment with a known hole or a debug switch | F4: the name for anyone who names nothing is written by the default branch alone — or not at all |
| **A faulty state keeps running** | An image with a fixed fault is published; the operator "rolls out", the name is unchanged, the runtime takes the old image from its local cache; the hole stays open, and everyone believes it is closed | F7: explicit pull and a call that reports the state, compared with the expected one; F6: both are in the deployment file |
| **A secret in the build log** | The smoke test works with a real access "so that it is realistic"; on a failure the container's log is printed, and the build log is readable for more people than the data store | F2: only throwaway values and a throwaway state location in the smoke test; a secret the service generates there is worthless afterwards |
| **The build credential with write access to the registry** | The build pipeline may publish; a step before publishing is skipped or is green although it checks nothing | F2: publishing happens only after the smoke test, in a step of its own afterwards; the smoke test must be able to turn red — a run of every assertion with its safeguard removed shows whether it does. The visibility measurement (F5) runs without the build credential |
| **Credentials for a problem that is none** | A private image answers "unauthorized"; the operator creates credentials, hands them out, stores them in the deployment file — and the image stays private | F5: visibility is measured anonymously; F6: the deployment file says that no login is needed and how that is known |
| **An image that should not be public** | The organization allows public images, a setting is changed, and an image with a built-in secret can be fetched from outside | F5 in the opposite direction: the same anonymous fetch shows it. And above all: no secret in the image — state and secrets are outside; that is a precondition of this concept (see the preconditions below) |
| **Someone else's image under the expected name** | A moving name is overwritten by someone who has or gains write access to the registry; the operator pulls something else next time | F3 and F6: anyone who wants to pin a state names an immutable name or the digest, and the deployment file says how. Against an attacker with write access to the registry this concept does **not** help beyond that — signature and provenance are not its subject (see "What it does not solve") |
| **A local build with external base layers** | The deployment file builds on the target machine; the base layer comes from the day of the build, and every machine has a different one; a hole is closed on one machine and not on the next | Principle of `service-as-single-artifact`: the deployment file names a published image. F6: the exception says so in its first line, with a reason that is maintained |
| **An outer edge that shuts out the wrong clients** | The proxy advertises a protocol the network does not let through; the attentive clients fail, and fault-finding ends at an invented block | F8: measure advertisements from outside, with a client that follows them; switch off what does not get through |

**What the concept does not protect against.** It does not check the provenance of an image: anyone who has write access to the registry can make a moving name point to something else, and against that only immutable names at the operator's and a signature help, which this concept does not cover. It does not check the content of the base layers for known holes. It does not protect against a compromised build environment — anyone who can change the build pipeline also changes the smoke test. And it does not protect against an operator reading the deployment file and rolling out without a check anyway; it only makes sure they hold everything they need not to.

## What it costs

This section lists what the concept costs. Every item is pinned to the rule that causes it and stands here without a counter-calculation.

**The build takes minutes instead of seconds and needs a container runtime (F2).** The image is started, its health check is waited for, it is restarted, removed and created anew, and after every step the test waits again — with a check that runs every thirty seconds, that is quickly two minutes and more per build, on every build. The build environment must be able to start containers and bind ports, and the build log gets longer and noisier. Anyone who builds many small changes pays this every time.

**The smoke test is code that needs upkeep (F2).** Every new interface, every new port, every new assurance of the service demands a line in the smoke test, and a forgotten line does not show: the smoke test is then green and checks less than it should. And every assertion has to run once with its safeguard removed and turn red, otherwise it is none.

**Two builds or a handover by digest (F2).** If you build the image into the local runtime for the smoke test and once more for publishing, you have two builds whose sameness is guaranteed by the build environment's cache, not by the procedure. If you instead build once and carry the digest to publishing, you need a build pipeline that can do this. Both cost; the author has the first way (see the evidence; which way is required is not yet settled).

**The naming rule is one more piece of configuration (F3, F4).** Computed names come from a tool or a rule that someone understands and maintains; the split into moving and immutable names fills the registry with one name per build, and someone has to clean up at some point. The promise `latest` costs keeping it: the default branch must be the recommended state, otherwise the name becomes a lie or has to be left out.

**The visibility measurement is a step without the build credential (F5).** It needs a place that is not logged in — a step of its own, a fresh machine, a call that explicitly does not use the stored credentials. That is inconvenient, because everything else in the build pipeline is logged in, and so it tends to be written as a sentence into a header instead of being built as a step (see the evidence).

**The deployment files age (F6).** Every name in the header, every order of steps, every sentence about what travels over the wire has to be updated on every change to every variant. A header that names a name that no longer exists is worse than none. And the header gets long: what it has to answer does not fit into three lines.

**The operator has to ask a question after every rollout (F7).** One action more, and one that only yields a result when something went wrong; it is therefore dropped as soon as it has said "correct" three times. And the question needs a way to the running instance: a one-off invocation in the container or an authenticated surface on the network — without either there is no answer, and the operator without a shell faces exactly this gap (not yet settled).

**Checking the outer edge needs a client that follows the advertisement (F8).** Such a client is not the one you habitually check with; it has to be obtained or built, run from outside the network, and the measurement repeats with every change to proxy or network. If you omit it, you notice nothing — until the first attentive client comes.

**The builder loses control over the timing (F1).** In a pull, the operator decides when to pull. A fixed fault is published and still keeps running somewhere, and the builder can neither enforce nor see it. That is the price of not needing access to the target.

## Where it applies

### Preconditions

**A deliverable that is built as an image and reports its build state.** Without the build stamp, F7 cannot be met, and F2 loses its last assertion. You recognize the situation by the fact that the question "which state is running?" gets only a name or a date as its answer.

**A build service that can start containers.** The smoke test needs a runtime in the build, ports and a few minutes. You recognize the situation by whether the build environment can start a container and query its health check.

**A registry that the target machine can reach.** The whole model presupposes that the operator can pull. You recognize the situation by the question whether the target machine may go out to the network.

**A container runtime at the operator's that understands a deployment file and allows a one-off invocation.** F6 presupposes the file, F7 the way to the running instance.

**State and secrets outside the image.** An image that carries a secret must not be public, and an image that carries state does not survive F2 — the recreation in the smoke test requires that nothing that must remain is inside the container. You recognize the situation by whether a version change takes anything with it.

**Exactly one default branch that is the recommended state.** F4 depends on it. You recognize the situation by whether the builder runs the default branch themselves.

### When this no longer holds

**The target machine cannot reach a registry.** This means an isolated network. The concept no longer holds because there is no pulling then; the substitute is an export of the image as a file, a transport and an import — and the deployment file then names the imported name, as a named exception (F6). F2 to F5 still apply, F7 as well; only the registry disappears. You recognize the situation by the question how the image gets onto the machine.

**An orchestrator brings its own rollout.** This means a platform that pins images by digest, enforces the pull itself and shows the running state. The concept holds only in part: F7 is then done by the platform, F6 becomes the description it understands; F2 to F5 stay unchanged, because they come before the platform. You recognize the situation by the fact that nobody starts a deployment file by hand any more.

**There is a channel to the target, and it is to be used.** This means an operator who gives the builder access, or a builder who serves their own machine and has a script. The concept no longer holds because this is then a push deployment and the alternative `push-deployment-that-keeps-its-secrets` applies; F2 to F5 stay useful nonetheless, and what F6 and F7 ask for, the script has to provide there. You recognize the situation by the fact that a script places something at the target.

**Many unknown operators to whom nothing can be explained.** This means an image pulled by hundreds who do not read the deployment file. The concept no longer holds because F6 and F7 rely on an operator who reads a file and asks a question; then signatures, fixed digests and a runtime that enforces both are needed — things this concept does not cover. You recognize the situation by not knowing who pulls.

**The service needs a secret from outside at first start.** This means a service that does not go into operation without a secret set beforehand. The concept still holds, but it has a gap: in a pull nobody can place a secret into it, and this concept does not say how it gets out (below).

### What it does not solve

**The first secret in a pull.** In a push, the script places the first secret; in a pull, only the service itself is inside, and its only back channel is its log. How a pulled service brings its first administration secret to the operator without anyone having a shell is outside the scope of this concept and is only named here as a gap.

**Signature and provenance.** That an image comes from the party that claims to build it and was not changed on the way, this concept does not check; F3 gives the operator only an immutable name with which to pin what they have checked once (not yet settled).

**Vulnerabilities in the base layers.** Whether the image's base layer carries known holes is a check of its own in the build and is not covered here.

**Rolling back and data migrations.** How an operator takes back a failed state — the immutable name of the previous build is the precondition, not the procedure — and what happens to the state when a version has changed it, this concept does not say.

**The availability of the registry.** If the registry cannot be reached, nobody can pull; whether an operator needs a mirror is operational work outside the scope of this concept.

**Cleaning up the registry.** One immutable name per build (F3) accumulates; when and how old images are removed without deleting a state that someone still pins is open.

### Not yet settled

* **Must the published image be the one that passed the smoke test — by digest, not by cache?** The reference builds twice and relies on the build environment's cache delivering the same image. That is true in practice and not guaranteed by the procedure: a build step that fetches something from the network on its second run can produce a different image. A build that builds once and carries the digest to publishing rules this out and costs a more complicated build pipeline. Whether F2 should require it is open.
* **How is F8 checked in general?** The incident concerned an advertisement through `Alt-Svc` and a UDP protocol. The rule says "with a client that follows the advertisement" — but which advertisements exist besides this one, which clients follow them, and can this be automated as a step of a deployment instead of a measurement by hand after the first break? Until it is answered, F8 is a lesson with a check that nobody has built yet.
* **Which kind of name does the operator get by default?** F6 requires the deployment file to state the kind of its name and explain the switch; it does not say which kind is the default. A moving name follows the builder and needs F7; an immutable one pins a state and needs an edit to the file for every version. For the sole operator the first is more convenient, for anyone who runs acceptance the second is safer. Whether this concept should recommend a default or whether it remains a question per operating situation is open.
* **Where does the question from F7 go when the operator has no shell?** F7 needs a way to the running instance: a one-off invocation in the container or an authenticated surface on the network. The operator without a shell does not have the first, and the second belongs to a design outside this concept; the reference answers the question today only through its administration surface and its administration secret. Does F7 need a third way — the stamp in a response that only a valid access gets anyway — or is this the limit at which operating without a shell stops working for the delivery too?
* **Should `latest` be written at all?** F4 binds the name to the default branch and says, for the case that it is not the recommended state: then do not write it. The reference shows that this case is the normal case of an implementation under construction — the default branch lags weeks behind the work. Would the more honest rule be never to write `latest` and always have the deployment file name a name? Then anyone who names nothing would get an error instead of a wrong state.
* **Does the signature of the image belong in this concept?** F3 gives the operator an immutable name; it says *which* build they pin, not *whose*. Against an attacker with write access to the registry only a signature that the operator verifies helps. That is a chain of its own with its own keys and its own incidents; whether it is a link of this chain or a concept of its own is open.
