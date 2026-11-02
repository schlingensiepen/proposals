---
type: Proposal Evidence
description: Where the concept stands; a worked example from invitation to machine access in an invented inventory system, the absence of a reference implementation, and the origin.
---

# Evidence

## Worked example: an inventory system

> **Example** (an inventory system; names and values invented), from the invitation to the machine access.
>
> **Inviting.** An administrator creates a user "Anna" with her address and generates an invitation. Out comes a link:
>
> ```
> https://…/#invite=k7Qx2mVd9pLw
> ```
>
> Everything after the `#` the server never sees (F9).
>
> **Redeeming.** Anna opens the link on her phone. The application reads the token from the fragment and sends it **once**; the server creates a device, asks for a label (Anna types "Anna's phone") and answers with a cookie it sets itself. From now on the invitation token is meaningless *for this device* (F3).
>
> In the evening she opens the same link on the tablet. A **second** device comes into being with its own credential, "kitchen tablet" (F4). Both devices record which invitation they came from; the administration therefore shows not "two redemptions" but **these two devices** (F3).
>
> **Losing.** The tablet disappears. The administrator revokes **that one** device; Anna's phone keeps working, and the history shows what was changed from the tablet (F4). On the tablet's next access the check ends at step 2, immediately, without any credential having to expire (F5).
>
> **Clearing up.** Because Anna's invitation lapses after the configured deadline anyway, nobody has to remember to withdraw it. If Anna later needs a third device, the administrator creates **a new invitation for the existing user**, not a second "Anna" (F2).
>
> **Rights.** Anna is granted the administrative right. She need not sign in anywhere anew; on her next access the server looks the right up at the user and finds it, on both devices (F5). On withdrawal the same applies, immediately.
>
> **Machine.** A computer is to back up at night. It gets **no** account but a capability key, like this:
>
> The deployment script writes a file next to the configuration on its run, say `setup-config.php`, with two values:
>
> ```php
> <?php return [
>   'setup_token' => '<setup-secret>',   // the same secret the operator holds
>   'valid_until' => '2026-08-15T19:42', // one hour after the run
> ];
> ```
>
> While the clock is below that, the setup procedures are open. In this window the operator has the API create **all** required machine keys: one with the task *backup* for the backup machine, one with the task *analyze* for the analysis machine, each named after its machine (F6). Each clear text appears **exactly once** and moves into that machine's configuration. Then the hour elapses and the same calls answer that the window is closed (F7, F8).
>
> **Anyone who could write that file had access to the machine anyway**: the window gives them nothing they did not already have.
>
> If a third machine joins half a year later, that is no special case: deploy, create its key in the then-open window, done.
>
> **Revoking, by contrast, is always possible.** If the analysis machine is compromised, an administrator shuts its key down **immediately**, without a deployment, without a window (F7). The backup key is untouched; and because it carries the task *backup*, the entire dataset was never reachable from the analysis machine in the first place.
>
> **The mistake this model prevents:** the user list contains no entry called "backup service".

## Reference implementation

**Omitted, because none exists yet (status: draft).** The model is fully decided and specified; the running application it comes from does not implement it: it predates this and had an anonymous reading stage, that is, exactly the state F1 excludes. The statements in this concept are therefore **thought through, not proven**; in particular the claim in F3 that visibility instead of prevention suffices in practice is so far an argument.

## Origin

The concept comes from the application described above, which serves a small, known circle. During writing it was corrected several times: the invitation link was first described as single-use and then recognized as the opposite of the core thesis, so being redeemable on every device became the purpose (F3); the counter on the invitation gave way to the origin recorded at each device (F3); the second invitation procedure was added because an expiry otherwise produces duplicates of the person (F2); the asymmetry between creating and revoking machine keys was made explicit (F7); and F10 was written as requirements on a rights system rather than as a rights model.
