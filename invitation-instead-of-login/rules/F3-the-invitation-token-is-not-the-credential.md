---
type: Proposal Rule
description: The invitation token permits only being exchanged, any number of times; each exchange creates a device with its own server-set credential, every device records its invitation, and invitations can be withdrawn and should expire.
number: F3
binding: must
role: load-bearing
applies_to: [builder]
---

# F3 · The invitation token is not the credential, and every device knows where it came from

**Applies to:** builder.

**What holds.** The token in the invitation link permits **exactly one** thing: being exchanged, and that **any number of times**. Every exchange creates another device with its **own** credential. The credential comes into being only then, is set by the **server**, and exists only there and in the browser.

**This is not a tolerated side effect but the purpose.** A human has a phone, a tablet and a computer. Were the link single-use, they would need three invitations for three devices: three mails, three procedures, three times the question of whether it has been redeemed yet. This way **one link suffices for one user and all their devices**, and because every exchange leaves its own credential, each device stays individually revocable (F4). The invitation is thus a statement about the **person**, "you belong", not about an endpoint.

**Every device records which invitation it came from.** The invitation itself keeps **no counter**. Together this yields the information that matters: *which* devices came out of a link, and that can actually be acted upon.

Two procedures must exist alongside:

| Procedure | Effect |
|---|---|
| **Withdraw the invitation** | The link can no longer be redeemed. Devices already created remain **untouched** and are revoked individually |
| **Expiry of the invitation** (recommended) | The link lapses by itself, even if nobody withdraws it |

**Why.** Were the link to carry the credential itself, the session secret would be permanently in a mail: in the mailbox, in the mailbox backup, in every forward. And everyone opening the same link would share a single credential; devices would then be indistinguishable, and F4 rests on exactly that.

**Why the origin is at the device and not as a number on the invitation.** A counter says "four devices came out of this link" and leaves you there. You can only act once you know **which four**: only then can they be inspected, named, revoked individually. With the origin at the device, count and last redemption are derivable anyway; a field of its own would be a second truth that can drift. (The trail reaches as far as the devices do: a **deleted** device takes it along, one more reason to make revoking the rule and deleting the exception.)

**The expiry is a recommendation, not a requirement**, but an emphatic one. A link without expiry stays in a mailbox, and mailboxes outlive devices, people and projects. An expiry costs nothing but the second procedure from F2: anyone who should still get in after the deadline receives a new invitation, no new account, no duplicate. **This is precisely why the two procedures in F2 are not a convenience but the condition under which an expiry is practicable at all.**

**Being redeemable more than once is a deliberate decision against the stricter variant.** A single-use link would force a new procedure for every further device of the same person: three devices, three mails. The security gain would be small, because anyone who has the link has it anyway; the real risk is the forwarded link, and single use prevents that only the first time.

**The interplay of the three levers**, and that there are three is the point: the **expiry** limits how long a link can work at all. **Withdrawal** ends it early when it has fallen into the wrong hands. **Revoking individual devices** clears up what it has already produced. None replaces another: withdrawal does not work retroactively, revoking does not work preventively, and an expiry does not help against the link forwarded today.

**Considered and rejected.** *The link carries the credential*: saves the exchange and puts the session secret permanently into a mailbox. *Single-use*: stricter, but expensive in daily life and ineffective against the actual case. *Keeping a counter on the invitation instead of the origin at the device*: answers "how many", not "which"; you then know something is wrong but can do nothing. *Doing without the expiry, "the invitation does get used"*: it gets used once and then stays valid for years in a mailbox.

**How you notice it is violated.** A session secret can be copied out of a mail, or you see that more devices came out of an invitation than expected and cannot say which they are.
