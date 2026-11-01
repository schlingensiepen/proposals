---
type: Proposal Agent Guide
name: invitation-instead-of-login
description: Self-contained guide for an agent building or reviewing access control for an application serving a small, known circle; invitations, per-device credentials, cookie handling, revocation, human identity versus machine capability, and the bootstrap window.
---

# Agent guide

## Purpose

You are building or reviewing access control for an application whose users are a small, known circle, with no registration and no passwords. This covers invitations, per-device credentials, cookie handling and longevity, revocation, the split between human identity and machine capability, and the bootstrap window in which machine keys are created. This guide is complete on its own; every rule, step and checklist item is written out here. Rules are named in parentheses as provenance; you can act correctly without opening the rule files.

Access arises from an invitation. One link serves all of a person's devices: it is redeemed once on each and leaves behind its own credential there, individually revocable. Humans get identity, machines get capability, and machine keys come into being only during a bootstrap window that a deployment opens.

### When to apply

- You are designing or reviewing access control for an application whose users are a small, known circle: family, club, team, a single operator with helpers.
- You are being asked to add "a login" to such an application.
- You are deciding how a background process, a backup job or an analysis service gets access.
- You are reviewing where session credentials are stored and how long they survive.

**Do not apply** when any of these hold: anyone should be able to sign themselves up; the circle grows beyond what one person can survey; a second factor is required by regulation; or credentials must be short-lived, for example on shared computers. Each makes the rules below wrong rather than merely inconvenient.

## Rules (MUST / NEVER)

MUST:

1. Require a credential for **every** access to content, reading included. There is no preliminary stage where possession of a hard-to-guess address suffices. (F1)
2. Create access through an invitation issued by someone with the right to do so, never through self-registration and never through a password. (F2)
3. Provide **two** separate procedures: (a) admit somebody new, which creates the user *and* their first invitation; (b) create a new invitation for an **existing** user, without creating a second user record. Without (b), every re-request produces a duplicate person, and rights, history and revocation end up spread across two records. (F2)
4. Make the invitation link redeemable **any number of times**. Each redemption creates another device with its own credential, so one link serves all of a person's devices. Redeeming happens once per device; do not confuse that with the link being single-use. (F3)
5. Record on every device **which invitation it came from**. Keep no counter on the invitation: a count answers "how many", the origin answers "which", and only the latter can be acted upon. (F3)
6. Provide a way to **withdraw** an invitation, which prevents further redemptions and leaves already-created devices untouched. (F3)
7. Give invitations an **expiry**. It is the only lever that works without anyone paying attention; a link without one stays valid for years in a mailbox. This is what makes rule 3(b) mandatory rather than optional. (F3)
8. Give every device its own credential, with the record representing **user on device**: the person carries name, address and rights; the device carries the credential and its label. (F4)
9. Store the credential in a cookie the **server** sets via a response header, with `HttpOnly` and `Secure` and a forwarding rule that withholds it on foreign-initiated calls. Never in storage the client manages. Two reasons: the client cannot read it, and, the one usually forgotten, browsers clear script-created storage after a few days without use, so a script-managed credential locks everyone out after a week away. (F4)
10. Renew the credential on every visit, and expect the browser to cap its lifetime at roughly 400 days whatever value you send. (F4)
11. Have the label assigned by the **user** at redemption, not by the inviter. (F4)
12. Keep the browser's full identification string and the last network address seen, the latest value only, no history, as a **recognition aid** for whoever reads the device list. It is not a credential and not an identifier. Expect one machine to appear as several devices: each browser profile has its own cookie store. (F4)
13. Put in the credential nothing but an opaque random reference, and store only its hash. (F5)
14. On every access, run these four steps in this order: resolve the credential → check the device is not revoked → check the user is not deactivated → check the right. Run the right check **before** resolving any domain identifier, or the error message alone reveals whether an object exists. (F5)
15. Give machines a key permitting **one task at one place**, representing no person: one key per task and per machine, named after both. The key *is* the scope; there must be no parameter with which the caller chooses it. (F6)
16. Create machine keys **only** while a bootstrap window is open, showing the clear text exactly once and storing only a hash. The window opens at **every** deployment, so connecting a new machine means: deploy, create its key in the same move. (F7)
17. Allow a machine key to be **shut down at any time** during normal operation, without a window and without a deployment. Creating may have hurdles; revoking must not. (F7)
18. Open the bootstrap window by writing a file next to the configuration containing the setup secret and an expiry timestamp, checked against the clock. Keep that secret **outside** the data store, because it is needed exactly when the store is empty, damaged or mid-migration. (F8)
19. Deliver any secret that must travel in an address, invitation token or entry key, in the **fragment** (`…/#invite=<token>`). The browser does not send it to the server. Read it in the client and send it once in a request body. (F9)
20. Hang domain rights on the **user**, not on the device or the credential. (F10)
21. Make the model's own procedures subject to rights: inviting, withdrawing an invitation, revoking a device, deactivating a user. (F10)
22. Let anyone revoke **their own** device without a special right: that is signing out, and it is the only remedy available to the affected person when a device is lost and nobody is reachable. (F10)
23. Prevent the last administrator from removing their own administrative right. Otherwise the system locks itself out and the only way back is a deployment. (F10)

NEVER:

- Never offer a password, a sign-in form or self-registration. (F2)
- Never put the session credential into the invitation link. It would then sit permanently in a mailbox, and all devices opening that link would share one credential. (F3)
- Never share one credential across a person's devices; a lost device could then only be handled by locking out the person. (F4)
- Never keep the credential in script-accessible storage. (F4)
- Never write rights into the credential. A right that travels with it holds until it expires, so a withdrawal takes effect months later. If you cache the lookup, say so: the promise is then "withdrawal takes effect within N minutes", not "immediately". (F5)
- Never give a machine a user account. It produces an identity that belongs to nobody, never expires, and that no audit catches because it looks like all the others. (F6)
- Never use one key for several machine tasks. The backup access is the only one that hands out everything at once. (F6)
- Never allow machine keys to be created from the running application, not even by an administrator; a captured administrative device would otherwise mint permanent access that survives the device being blocked. (F7)
- Never bind revoking to the bootstrap window "for symmetry": a compromised machine would keep running until the next deployment. (F7)
- Never store a machine key's clear text so it can be displayed again. (F7)
- Never rely on a setup file that somebody must delete after installation. (F8)
- Never put a secret in a query string or path segment. (F9)
- Never ship a rights model with this concept; the cut of domain rights belongs to the application. (F10)
- Never treat inviting as a "small" right beside administering: whoever may invite grants access to everything. (F10)

## Procedure

**Before you build: decide the invitation deadline with the user, never on your own, and record it.** Rule 7 requires an expiry, but no number is prescribed: too short and the inviter constantly sends new links, too long and the expiry is a reassurance without effect. Ask how quickly invitees in this circle actually react, propose a deadline, and record the decision with its reason. (F3)

**Admitting a person:**

1. Create the user with name and address.
2. Create an invitation for that user, with the recorded expiry.
3. Deliver the link with the token in the fragment.
4. On each device the person opens it: read the token from the fragment, send it once in a request body, create a device record, ask the person for a label, set the cookie, and store which invitation the device came from.

**Losing a device:**

1. Revoke that device. Do not touch the person or their other devices.
2. If the invitation is still live and may have leaked, withdraw it as well; this stops new redemptions but changes nothing about existing devices.
3. Consult the history for what was done from that device.

**Connecting a machine:**

1. Deploy. The deployment writes the setup file with secret and expiry.
2. While the window is open, have the API create one key per task for that machine, named after machine and task.
3. Take each clear text once and write it into that machine's configuration.
4. Let the window lapse; do not extend it manually.
5. If the machine is later compromised, shut its key down immediately: no deployment, no window.

**Reviewing an existing application against this concept:**

1. Try every content route without a credential; anything that answers with content fails. (F1)
2. Grep for password fields, sign-in forms and registration routes. (F2)
3. Check that an invitation can be redeemed twice and produces two device records. (F3)
4. Check that each device record names its invitation. (F3)
5. Check that the cookie is set via a response header, not by client code. (F4)
6. Withdraw a right and confirm it stops working on the next call, on every device. (F5)
7. List the user records and look for entries with no human behind them. (F6)
8. Try to create a machine key with the window closed; it must be refused. (F7)
9. Try to revoke a machine key with the window closed; it must succeed. (F7)

## Acceptance checklist

- [ ] Does every content route require a credential, reading included? (F1)
- [ ] Is there no password, no sign-in form and no self-registration? (F2)
- [ ] Do both invitation procedures exist, admit new and re-invite an existing user? (F2)
- [ ] Can one link be redeemed on several devices, each producing its own credential? (F3)
- [ ] Does every device record which invitation it came from, with no counter on the invitation? (F3)
- [ ] Can an invitation be withdrawn, and does it expire by itself after the recorded deadline? (F3)
- [ ] Is the credential set by the server via a response header, `HttpOnly` and `Secure`? (F4)
- [ ] Is it renewed on every visit? (F4)
- [ ] Are the identification string and last network address kept as the latest value only, and treated as a recognition aid rather than an identifier? (F4)
- [ ] Is the credential stored only as a hash, carrying no rights? (F5)
- [ ] Do the four checks run in order, with the right check before any identifier is resolved? (F5)
- [ ] Does revoking a device take effect on the next access, and deactivating a user on all their devices at once? (F5)
- [ ] Does every machine have its own key per task, named after machine and task, with no scope parameter? (F6)
- [ ] Does the user list contain no entry belonging to nobody? (F6)
- [ ] Is creating a machine key impossible while the window is closed, and revoking one always possible? (F7)
- [ ] Does the window open at every deployment and close by itself? (F8)
- [ ] Is the setup secret outside the data store? (F8)
- [ ] Does no secret appear in a query string or path? (F9)
- [ ] Do domain rights hang on the user, are the model's own procedures rights-bound, can anyone revoke their own device, and is the last administrator protected? (F10)

## Common mistakes and their fixes

| Mistake | Fix |
|---|---|
| Invitation made single-use "for safety" | Make it multiply redeemable; otherwise a person with three devices needs three invitations, and the link stops being a statement about the person |
| Counter on the invitation instead of origin at the device | Record the origin at each device; a count tells you something is wrong but not what to revoke |
| Invitation never expires | Add an expiry, and the re-invite procedure that makes it survivable |
| Re-request creates a second user record | Add "new invitation for an existing user"; duplicates split rights and history |
| Session token kept in client-managed storage | Move it into a server-set cookie; script storage is cleared after days without use |
| One credential for all of a person's devices | One per device; otherwise a lost phone can only be answered by locking out the person |
| Rights encoded in the credential | Look them up per access; otherwise withdrawal takes effect months later |
| Background process given a user account | Give it a capability key named after machine and task |
| One key for backup and analysis | Split them; only the backup access hands out everything |
| Machine key creatable from the admin area | Move creation into the bootstrap window; keep revocation available always |
| Setup file that must be deleted afterwards | Replace with a file carrying an expiry timestamp |
| Invitation token in `?invite=…` | Move it into the fragment |
| Menu hidden in the client for non-admins, calls left open | The client hides nothing; only the server decides |

## Self-check

Before you report completion: can you name the recorded invitation deadline, where the setup secret lies outside the data store, the two anchors and which rights hang on each, every machine key with its machine and task, and who may invite, withdraw, revoke and deactivate? If any of these is missing, you are not done. If a rule seems wrong for your case, read that rule and the scope of this bundle before deviating, and record the deviation.
