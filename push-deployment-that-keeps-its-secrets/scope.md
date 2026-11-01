---
type: Proposal Scope
description: Security considerations, what the concept costs, where it applies, when the answer flips, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

| Danger | Typical course of events | Countermeasure |
|---|---|---|
| **Credentials in the repository** | Somebody enters address and password "just for testing" into a versioned file. They stay in the history, even after removal | F1: accesses lie exclusively in the instance directory ignored by version control. The ignore pattern covers **all** instance directories, not individual names; otherwise the second target gets checked in |
| **System secret in the package** | The production configuration lies in the working directory and is taken along when the folder is transferred | F3: what is transferred is the list of versioned files. Whoever transfers the folder instead distributes their own secrets |
| **Store overwritten** | An exclusion list is copied and shortened; on the next run a path is missing | F4: lock in the core of the script, not in the configuration. The mistake cannot be repaired |
| **`--force` without understanding** | A run fails, somebody appends `--force`, and the signing key is created anew: all addresses signed so far are worthless | F5: the switch **names what it destroys before it acts**. A prompt "overwrite?" shifts the decision onto somebody who does not know the consequence |
| **Permanent access from a captured administration access** | Whoever takes over an administrator's device issues themselves a machine access that survives being locked out | F6: such accesses arise only in the window, and can nevertheless be revoked at any time |
| **Window stays open** | It is opened by hand and nobody closes it | F6: fixed period, comparison with the clock, no manual closing provided for |
| **Open directory on the host system** | A protective file lies in an ignored folder and was never versioned; it is missing on the server | F3 (version the exceptions to the exceptions explicitly) **and** F9 (check from outside). Neither of the two suffices alone |
| **Instance directory lost** | New machine, deleted folder: the second backup of the signing key is gone | F2: the copy beside the instance configuration, with narrow permissions. It is the only protection; a data backup of the system does not include it |

**What the concept does not protect against:** it does not protect against a compromised host system nor against a compromised machine of whoever delivers; whoever can write there controls both. It does not replace rights management within the system itself; it only sets it up.

## What it costs

- **A second directory per instance that nobody backs up except the operator.** If it is lost, the system secrets backed up in it are gone, and with a signing key possibly everything it has signed.
- **A script of its own per component and transfer path**, plus a guide for manual operation for each (F10). That is more material than "a deploy script" suggests.
- **The first delivery is a special case.** It creates secrets and creates configurations; no later one does. Two behaviors in one tool, and the rarer one is the more dangerous.
- **`--force` exists.** So there is a way to destroy everything, and it is documented. That is the deliberate decision of F5, but it remains a way.
- **The order applies only the first time per instance**; afterwards it does not matter. Whoever memorizes it as an iron rule will be surprised later.

## Where it applies

### Preconditions

Several components that are delivered separately. An operator who owns and keeps the instance directories. Host systems onto which files can be transferred. System secrets that may come into being during delivery.

### When the answer flips

The cut becomes wrong when:

- a **secrets manager** exists that already does creation, storage and rotation; then F2 is superfluous and the instance directories shrink to addresses;
- the delivery runs **unattended** from a pipeline; then there is no human who uses a window or reads a `--force` warning, and F5 to F7 need a different answer;
- **immutable host systems** are used that are created anew with every delivery; then there is no existing configuration that would need protecting, and F4 and F5 lose their object;
- a component has **many instances of the same kind**; one hand-maintained directory per instance holds for a handful, not for a hundred.

### What it does not solve

How the application manages its rights: this concept sets up the access control, it does not design it. What a rollback looks like (undoing a failed delivery). How system secrets are changed **on a schedule**; F5 only creates the possibility. And how several people who deliver coordinate: one is assumed throughout.

### Not yet settled

* How long the right window is (F6). F6 names no number. Too short, and a slow transfer path lets it close before the last component had its turn; too long, and it stands open while nobody is watching. Presumably it depends on the duration of the longest run; that is not proven.
* What happens when the order is not linear (F7). F7 assumes that one component opens the window and the others use it. With two components that would have to give each other accesses there would be a cycle. Whether that is a design fault of the system or a case the concept has to handle is open.
* What a scheduled change looks like (F5). With `--force`, F5 creates the possibility of replacing a system secret but says nothing about the course of it: how to swap a key without locking out the components that still use it. Old and new values side by side are provided for nowhere.
* Whether the separation "script versioned, instance not" also holds when a team delivers (F1). One operator is assumed throughout. With several, the instance directories would have to be shared, and then they are again a shared place with secrets, for which this concept has no answer.
