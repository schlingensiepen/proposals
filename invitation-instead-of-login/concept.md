---
type: Proposal Concept
description: The problem of access for a small, known circle and its machines, the concept of invitation, per-device credential and machine capability, its load-bearing structure, and the procedures it introduces.
---

# The concept

**In one sentence:** There is no login form and no registration; anyone who comes in was invited and exchanges the invitation link **once on each device** for a credential of its own; machines, kept separate, receive keys that permit a task and represent no person.

**Abstract:** A password is a secret a human must keep, and a login form is the place where it can be guessed. Both disappear once access is a **procedure** rather than a property: someone invites, the invitee redeems, and from then on their device identifies them. **A single link suffices for all of a person's devices**; it is redeemed once on each and leaves behind a credential there that can be revoked individually. The price is that the same link also works in the wrong hands.

## Problem

"I am building something for a small, known circle: family, a club, a team. I want no registration, no passwords, no mail-based resets, no second factor. But I also do not want everyone who knows the address to read along. What I need is: I say who belongs, and after that it simply works."

Three questions hide in that, and they must be answered separately:

- **How does someone get in the first time**, without having to invent and remember something?
- **How does the server recognize them afterwards**, without them typing anything on every visit?
- **And what about the machines?** A backup process, an analysis service: they have no device, no person and no mail address, yet they need access.

The third question is almost always answered wrongly in this situation: the background process gets a user account. With that, an identity has come into being that belongs to nobody, never expires, and holds the same rights as a human.

## The concept

**There are no guests.** Every piece of content requires a credential, reading included, even with a hard-to-guess address in hand. Anyone who should belong gets an **invitation link**. It carries a token that permits **nothing but** being exchanged, and that **any number of times**: on opening, the application reads it out, sends it, and the server answers with a credential it sets itself. **One link therefore suffices for all of a person's devices**; on each it is redeemed once and leaves a credential of its own behind.

This credential belongs to **one device**, not to a person. The record behind it is a "user on device": the same person on phone and tablet has two credentials, two people on the same phone likewise. Only so can a lost device be blocked without hitting all the others. An invitation link is therefore **redeemable more than once**, every redemption creating another device, and **every device records which invitation it came from**. That makes it answerable *which* devices came out of a link, not only how many. The link itself has an **expiry** and can be **withdrawn**; both work only forwards, and what it has already produced is revoked individually.

**Rights hang on the user, not on the credential.** The server looks them up on every access instead of writing them into the credential; a withdrawal therefore takes effect immediately and not only when a cookie expires.

**Machines get something else.** No user account, but a key permitting **one task**, backing up or analyzing, and representing no person. Such keys do not come into being during normal operation but only during a **bootstrap window** opened by a deployment.

**Load-bearing structure.** **F1 is the precondition of everything**: without "no guests" the rest is a convenience rather than an access model. **F3 and F4 belong together:** the exchange exists so that each device can get a credential of its own; without F4, F3 would be superfluous, without F3, F4 unreachable. **F5 carries manageability** and is individually adoptable; it holds for any access model with long-lived credentials. **F6 is the boundary between the two worlds**, and **F7 and F8 are one component, not two rules**: F7 says where capability keys may **not** come into being, F8 provides the place where they do. Together they yield a practicable procedure: deploy, create while the window is open, the window closes by itself.

*For adopters:* if you take F7 without F8, you have a prohibition without an alternative place. The need does not disappear, the backup wants its key, so creation slides back into normal operation, silently. That is not a fault of this model but the way it breaks when adopted piecemeal.

**F9 is a transport matter** and independent of everything else. **F10 hangs on F2 and F4**: it names the responsibility for the procedures those two introduce; without F10 they exist, but nobody may carry them out. **If you take only one rule, take F6**: it prevents the mistake most commonly made in this situation.

## The procedure

The rules say what must hold. These are the procedures the concept introduces, as they occur in operation.

**Setting up a fresh system.** A fresh system has nobody who could invite. Deploy; the deployment writes the setup file with secret and expiry and so opens the bootstrap window. While it is open, and with the setup secret, run the setup procedure that creates the first administrator: a user with the right to invite and administer, and their first invitation, redeemed like any other. Create the machine keys needed now in the same window. Let the window lapse; afterwards the setup procedures answer that it is closed. (F2, F8)

**Admitting a person.** Create the user with name and address. Create an invitation for that user, with an expiry. Deliver the link with the token in the fragment. On each device the person opens it, the application reads the token from the fragment and sends it once in a request body; the server creates a device record, asks the person for a label, sets the cookie, and stores which invitation the device came from. (F2, F3, F4, F9)

**Losing a device.** Revoke that device; do not touch the person or their other devices. If the invitation is still live and may have leaked, withdraw it as well; this stops new redemptions but changes nothing about existing devices. (F3, F4, F5)

**Connecting a machine.** Deploy; the deployment writes the setup file with secret and expiry. While the window is open, have one key per task created for that machine, named after machine and task. Take each clear text once and write it into that machine's configuration. Let the window lapse; do not extend it by hand. If the machine is later compromised, shut its key down immediately, without a deployment and without a window. (F6, F7, F8)

**Reviewing an existing application.** Try every content route without a credential; look for password fields, sign-in forms and registration routes; redeem one invitation twice and check for two device records, each naming its invitation; check that the cookie is set by a response header; withdraw a right and confirm it stops working on the next call, on every device; look for user records with no human behind them; try to create a machine key with the window closed (refused) and to revoke one (succeeds). (F1 to F8)
