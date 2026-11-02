---
type: Proposal Rule
description: Every device gets its own credential and the record is "user on device"; the credential lives in a server-set cookie, and "device" in truth means browser profile.
number: F4
binding: must
role: load-bearing
applies_to: [builder]
---

# F4 · One credential per device: the record is "user on device"

**Applies to:** builder.

**What holds.** Every device gets its own credential. The record behind it describes neither a person nor a device but the connection: **this user on this device**. The person carries name, address and rights; the device carries the credential and its label.

**Why.** What you cannot tell apart you cannot block individually, and blocking individually is the only procedure that counts when a device is lost. A shared credential across all of a person's devices forces you, in an emergency, to lock out the person instead of the device.

The separation is also the only one that models daily life correctly: a person has several devices, and a device is used by several people. Only "user on device" carries both. It incidentally enables information that would otherwise be missing: the history can record **who** did something **and from which device**, the decisive question with a lost device.

**Where the credential lives: in a cookie the server sets.** Not in storage the application manages itself, and that has **two** reasons, of which usually only the first is named:

1. **The client cannot read it** (`HttpOnly`). A flaw in the browser code cannot reach it, and it does not end up in client-side logging by accident.
2. **It survives.** Modern browsers clear storage created by **script** after a few days without use; tracking protection makes no distinction between tracking and signing in. A cookie the server sets via a response header is exempt and keeps its lifetime. If you put the credential into script storage, you build a model that locks everyone out after a week of vacation.

Three values and one trap you must know:

| | |
|---|---|
| Lifetime | long, but **capped**: browsers limit it to roughly 400 days, whatever value is sent. After a good year a fresh redemption is needed anyway |
| Renewal | on every visit, so a daily-used device never falls out mid-use |
| Attributes | `Secure`, and a forwarding rule that withholds the credential on calls initiated by another site |
| **Trap** | If your address points via an alias at a **third-party** host, some browsers treat even server-set cookies like script storage and cap them to a few days. Longevity then hangs on name resolution, not only on code |

**"Device" really means "browser profile", and that must be said out loud.** A credential lives in a cookie store, and a machine has several: anyone who opens the same link in two browsers gets **two** records for **one** piece of hardware. The same goes for separate profiles, private windows and work profiles. This is not a fault of the model, it counts accesses, not hardware, but it surprises everyone seeing the list for the first time.

**Hence the recommendation: keep the browser's full identification string**, which it sends on every call anyway, along with the last network address seen, **the latest value only, no history**. It is **not a credential** and not an identity; it is a recognition aid for the human sitting in front of the list who must decide which of the two entries called "office computer" was the Firefox profile. Without it the same name appears twice and nobody dares to revoke.

**The label is assigned by the user at redemption**, not by the inviter. They know better which device they are holding; a label guessed by an administrator is worthless after the second device. It remains **authoritative**: the identification string helps with recognition but decides nothing.

**Considered and rejected.** *One credential per person*: simpler, and it makes blocking a device impossible. *One credential per device with no person behind it*: then nobody has an address through which to obtain a new one, and the same person appears repeatedly with no discernible connection. *Recognizing devices automatically* instead of naming them: technical characteristics are unreliable and change with every update; they serve as a memory aid, not as an identifier. *Keeping the credential in script storage*: looks tidier, forfeits `HttpOnly` **and** longevity. *Merging several browsers of the same machine*: there is no reliable characteristic for it, and the attempt confuses accesses with hardware, exactly the distinction that counts when revoking.

**How you notice it is violated.** A lost device can only be blocked by locking somebody out.
