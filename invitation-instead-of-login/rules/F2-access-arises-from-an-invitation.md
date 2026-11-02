---
type: Proposal Rule
description: No sign-in form, no password, no self-registration; anyone who belongs is invited, and the application offers two procedures, admitting somebody new and re-inviting an existing user.
number: F2
binding: must
role: load-bearing
applies_to: [builder]
---

# F2 · Access arises from an invitation, not from registration

**Applies to:** builder.

**What holds.** There is no sign-in form, no password and no self-registration. Anyone who should belong is **invited** by someone with the right to do so.

**Why.** A password moves the system's security into users' heads and password managers, and immediately produces the follow-on problems that make up the greater part of any sign-in solution: resets, minimum requirements, reuse, second factor. A sign-in form is also the place where a stranger can make attempts; without a form there is nothing to guess.

The real gain, though, is another: **with an invitation, belonging is a decision somebody makes**, not a state somebody claims. For a known circle that is the more honest model: there is nobody who "should be able to sign up", only people who belong.

**Considered and rejected.** *Username and password*: familiar, and you buy the whole appendix with it: resets, lockout logic, requirements on the choice. *Sign-in through a third-party provider*: spares you the safekeeping but makes access depend on a third party and assumes everyone has an account there. *One-time codes by mail on every visit*: solves the same problem and demands a mailbox switch on every access; that works for rare use, not for daily.

**What this demands of the application: two procedures, not one.** Inviting and creating a user are **not the same**, and if you merge them, you notice only when the first invitation has expired (F3):

1. **Admit somebody new**: creates the user *and* their first invitation.
2. **Create a new invitation for an existing user**, without creating a second user. This is the normal case after day one: expired link, lost device, one more device.

Without the second procedure, every re-request creates **a duplicate of the person**, and rights, history and revocation are then spread across two records nobody ever merges. Self-service ("request a new access link") is that same second procedure, merely triggered by the affected person instead of the inviter.

**How you notice it is violated.** There is a page where somebody can type a password, or the same person appears twice in the list because they needed a new link.
