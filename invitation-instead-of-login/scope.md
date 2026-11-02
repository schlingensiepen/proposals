---
type: Proposal Scope
description: Security considerations, what the concept costs, where it applies, when this no longer holds, what it does not solve and what is not yet settled.
---

# Scope

## Security considerations

| Threat | Typical course | Countermeasure |
|---|---|---|
| **Forwarded invitation link** | The link is forwarded or stays in a mailbox somebody else gains access to. It is redeemable more than once, so further devices come into being | Three levers that do not replace one another (F3): the **expiry** limits preventively, **withdrawal** ends it early, **revoking individual devices** clears up. Because every device knows its invitation, it is visible **which** devices are affected, not just how many |
| **Old link in an old mailbox** | An invitation from two years ago is found and redeemed. Nobody remembers it, nobody withdrew it | The recommended **expiry** (F3). It is the only lever that works without attention; withdrawal presupposes that somebody thinks of the link |
| **Lost device** | A phone with a valid credential goes missing. The credential is valid for months | Individual revocation, enabled by F4. A history, where the application keeps one, can additionally record **what** was done from that device; without F4 neither would be possible |
| **Captured administrative access made permanent** | Anyone who takes over an administrator's device issues themselves a machine key. It survives the device being blocked and hands out the entire dataset | F7: machine keys come into being only in the bootstrap window. That is the reason for this rule, not a side effect |
| **Ownerless identity** | A background process gets a user account "like an employee". It has no address, no owner, no expiry, and nobody misses it | F6: machines receive capability, not identity. An account with no human behind it is caught by no audit, because it looks like all the others |
| **One key for everything** | Backup and analysis share a key, "it is the same service anyway". The analysis machine is in the living room | Separation per task **and** per machine (F6). The backup access is the only one that hands out everything at once |
| **Secret in the access log** | An invitation token is in the query string. It lands in the server log, the browser history and the `Referer` | F9: fragment or nowhere. This threat arises without an attack, through normal operation alone |
| **Right withdrawn, no effect** | An administrative right is withdrawn, but the credential on the device keeps working | F5: rights are looked up, not carried along |

**What the concept does not protect against:** it does not protect against a taken-over device; anyone who has it has the credential. It does not replace a rights check in the individual procedures; it only says **who** is calling, not **whether they may**. And it knows no second factor: possession of a device that has been set up is the only credential.

## What it costs

- **A forwarded link works until somebody acts.** Expiry and withdrawal work only forwards; what the link has already produced must be revoked individually. That is visible, but it is work.
- **The expiry forces a second procedure.** If you let invitations lapse, you need the function "new invitation for an existing user" (F2); otherwise duplicates of the person appear. That is not an addition but the price of the expiry.
- **No access without somebody who invites.** On a fresh system the bootstrap is the only way in; if the bootstrap secret is lost, only access to the storage helps.
- **Credentials are long-lived, so their loss has consequences.** The model stands and falls with a lost device actually being revoked; that never happens automatically.
- **Two worlds instead of one.** Humans and machines are administered separately. That is the point, but it costs two surfaces, two lists, two routes.
- **Looking up rights costs one access more**, on every call.
- **Without a password there is nothing a human can repeat.** Anyone who loses their device and has no way to obtain a new link themselves depends on somebody else.

## Where it applies

### Preconditions

A circle that can be **named**: somebody knows who belongs. Clients allowed to keep a cookie. A place that invites. A deployment that can open a bootstrap window.

### When this no longer holds

The model becomes wrong when:

- **everyone should be able to sign themselves up**: then belonging is no longer a decision, and the invitation is a facade;
- the circle grows so large that **nobody keeps track** of who was invited: the visibility from F3 substitutes for prevention only while somebody looks;
- a **second factor is required** by regulation: possession of a device that has been set up is exactly one;
- **credentials must be short-lived**, for instance on shared computers; this model lives on long-lived credentials and becomes an imposition with short ones.

### What it does not solve

This concept prescribes **no rights model**: which domain rights exist, how finely they are cut and who gets them is for the application to decide. What it does **not** leave open are the five requirements from F10 on any rights system built here; in particular, responsibility for inviting and revoking is not left to improvisation. It says nothing about the **shape of the interface** over which credentials travel, and nothing about the **delivery of the application** itself. And it describes no way to detect a lost device **automatically**; that remains a human decision.

### Not yet settled

* **How long the right deadline for an invitation is.** F3 recommends an expiry but names no number. Too short and the inviter constantly sends new links; too long and the expiry reassures without protecting. It probably depends on how quickly invitees actually react; experience is missing here. Until then the deadline is set at adoption and recorded (see the agent guide).
* **What happens if the bootstrap secret is lost.** F8 makes it the system's strongest secret and puts it outside the store. If it is lost, the way back is access to the storage. Whether that is the accepted answer, or whether a second, equally protected route is needed, is open.
* **How a device is recognized without measuring it.** F4 relies on a label assigned by a human, because technical characteristics are unreliable. With ten devices of the same person that helps little. Whether there is a recognition that does not rest on a fingerprint of the device is open.
* **Whether a capability key should expire.** F7 makes it a deliberate act but not a time-limited one. For capability keys in general an expiry is commonly treated as a duty of the operator. Here the question is sharper, because creation is bound to a deployment (F7): an expiry would force a deployment at a moment determined by the deadline rather than by operations. Whether that is a gain or a new source of error, and whether the answer would be a deadline *extended* at deployment time rather than one that strikes by itself, is open.
