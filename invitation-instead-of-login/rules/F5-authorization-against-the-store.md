---
type: Proposal Rule
description: The credential proves who, not what somebody may do; it is an opaque reference stored as a hash, and every access runs four checks in fixed order against the store.
number: F5
binding: must
role: load-bearing
applies_to: [builder]
---

# F5 · Authorization runs against the store, not against the credential

**Applies to:** builder.

**What holds.** The credential proves **who**, not **what somebody may do**. Rights sit with the user and are looked up on every access; they are not written into the credential.

**What is in the credential: nothing but a reference.** It is a random value without meaning: no role, no name, no bundle of rights, no built-in expiry. It is stored only as a **hash**: whoever captures the store gets no working credentials.

**What happens on every access**: four steps, and their order is part of the rule:

1. **Resolve the credential**: hash it, find the device. Nothing found: not signed in.
2. **Check the device**: if it is revoked, it ends here, whoever stands behind it.
3. **Check the user**: if they are deactivated, it ends here, and for **all** their devices at once.
4. **Check the right**: only now, and **before** resolving any domain identifier. Otherwise the error message alone reveals whether an object exists.

**Why.** A right written into the credential holds until the credential expires. With credentials valid for months, the normal case here, that means a withdrawal takes effect months later. Looked-up rights take effect on the **next access**, in both directions: withdrawal **and** granting, on all of the person's devices, without anyone having to sign in again.

From the same property follow two procedures that would otherwise not exist: revoking a **device** takes effect immediately, because step 2 finds it. Deactivating a **user** takes effect immediately on all their devices, because step 3 catches it there, without having to visit the devices individually.

**The credential is renewed on every visit.** Only therefore may it be long-lived without being permanent: a device unused for a year falls out by itself, a daily-used one never expires mid-use.

**The price is one lookup per access.** In a system that opens the store on every access anyway it is not measurable; it would be in an architecture built to avoid exactly that. **Whoever caches it buys back the delay they wanted to avoid**, in exactly the amount of the caching. That is permissible but must then be said out loud: "withdrawal takes effect within five minutes" is a promise; "withdrawal takes effect immediately" would then be false.

**Obvious and rejected.** *Write rights into a signed credential*: saves the lookup and is the reason revocation lists had to be invented; the revocation list is then the lookup, only clumsier and with a second truth beside it. *Short lifetimes instead of lookups*: the usual answer; it trades the withdrawal delay for a renewal procedure that can itself fail, and fits badly with devices unused for weeks. *A second credential for elevated rights* ("administration only on the home computer"): doubles the state you must keep consistent, for protection anyone with device access defeats anyway. *Storing the credential in clear, "it is only a session"*: a captured store is then a collection of valid accesses.

**How you notice it is violated.** After a right is withdrawn, it keeps working on one device.
