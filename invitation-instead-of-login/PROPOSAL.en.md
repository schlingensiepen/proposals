---
type: Concept
status: Draft
version: "1.0"
date: 2026-08-15
author: Jörn Schlingensiepen
slug: invitation-instead-of-login
language: en — translated from the German original
sync: v1.0 (2026-08-15)
question: "How does the server know who is calling — without a login form, without a password, and without guests?"
thesis: "Access arises from an invitation; one link invites a user for all their devices, because every redemption creates its own credential — and machines receive capability rather than identity."
---

# Invited, not logged in

**In one sentence:** There is no login form and no registration — whoever comes
in was invited and exchanges the invitation link **once on each device** for a
credential of its own; machines, kept separate, receive keys that permit a task
and represent no person.

**Abstract:** A password is a secret a human must keep, and a login form is the
place where it can be guessed. Both disappear once access is a **procedure**
rather than a property: someone invites, the invitee redeems, and from then on
their device identifies them. **A single link suffices for all of a person's
devices** — it is redeemed once on each and leaves behind a credential there
that can be revoked individually. The price is that the same link also works in
the wrong hands.

## 1. Problem

"I am building something for a small, known circle — family, a club, a team. I
want no registration, no passwords, no mail-based resets, no second factor. But
I also do not want everyone who knows the address to read along. What I need is:
I say who belongs, and after that it simply works."

Three questions hide in that, and they must be answered separately:

- **How does someone get in the first time**, without having to invent and
  remember something?
- **How does the server recognize them afterwards**, without them typing
  anything on every visit?
- **And what about the machines?** A backup process, an analysis service — they
  have no device, no person and no mail address, yet they need access.

The third question is almost always answered wrongly in this situation: the
background process gets a user account. With that, an identity has come into
being that belongs to nobody, never expires, and holds the same rights as a
human.

## 2. The concept

**There are no guests.** Every piece of content requires a credential — reading
included, even with a hard-to-guess address in hand. Whoever should belong gets
an **invitation link**. It carries a token that permits **nothing but** being
exchanged — and that **any number of times**: on opening, the application reads
it out, sends it, and the server answers with a credential it sets itself. **One
link therefore suffices for all of a person's devices**; on each it is redeemed
once and leaves a credential of its own behind.

This credential belongs to **one device**, not to a person. The record behind it
is a "user on device": the same person on phone and tablet has two credentials,
two people on the same phone likewise. Only so can a lost device be blocked
without hitting all the others.

**Rights hang on the user, not on the credential.** The server looks them up on
every access instead of writing them into the credential; a withdrawal therefore
takes effect immediately and not only when a cookie expires.

**Machines get something else.** No user account, but a key permitting **one
task** — backing up, analysing — and representing no person. Such keys do not
come into being during normal operation but only during a **bootstrap window**
opened by a deployment.

## 3. Terms

- **Credential** — a piece of evidence with which a caller shows who or what it
  is.
- **Identity credential** — a credential pointing at a **bearer** (this human on
  this device); what it may do is looked up.
- **Capability key** — a credential permitting a **task** without representing
  anyone; what it may do is contained in it.
- **Invitation** — a procedure producing a token whose sole purpose is to be
  exchanged for an identity credential.
- **Redeeming** — the exchange: invitation token in, identity credential out.
  **Once per device, any number of times per link** — do not confuse the two
  "once": a device needs the exchange exactly once, a link permits it without
  limit.
- **Device** — the record an identity credential hangs on; more precisely, "user
  on device". It records which invitation it came from.
- **Revoking** — rendering a single credential ineffective without affecting
  others. To be distinguished from **withdrawing** an invitation, which only
  prevents future redemptions.
- **Bootstrap window** — a time-limited state in which procedures are permitted
  that are forbidden during normal operation.
- **Fragment** — the part of an address after the `#`. The browser **does not
  send it to the server**; it appears in no access log and no `Referer`.

## 4. Decisions

### F1 · There are no guests

**What holds.** Every access to content requires a credential — reading
included. There is no preliminary stage in which possession of a hard-to-guess
address suffices for reading.

**Why.** A reading stage of "whoever has the address may look" seems harmless
and is the most expensive part of the system: it makes the unguessability of the
address the **sole** protection of everything reachable through it — with no way
to revoke, no traceability, no limit on trying. And it grows: what shows "only
the overview" today shows the history including every participant's name after
the next extension.

The price is lower than it seems. The alternative would be convenient if setting
up were laborious — but it consists of **one click**, which anyone makes anyway
who wants more than to look at a single page.

**Obvious and rejected.** *Reading free, writing protected* — the classic split,
and it assumes reading is harmless; with records carrying history, names and
locations it is not. *Unguessable address as access protection* — it protects
against trying out identifiers, it does not replace a credential; it cannot be
withdrawn and sits in every access log that ever saw it. *"Open for now, secure
it later"* — the securing never comes, because by then it would lock out
existing users.

**Symptom of violation.** There is an address at which content is visible
without a credential — even if "only" an overview.

### F2 · Access arises from an invitation, not from registration

**What holds.** There is no sign-in form, no password and no self-registration.
Whoever should belong is **invited** by someone with the right to do so.

**Why.** A password moves the system's security into users' heads and password
managers — and immediately produces the follow-on problems that make up the
greater part of any sign-in solution: resets, minimum requirements, reuse, second
factor. A sign-in form is also the place where a stranger can make attempts;
without a form there is nothing to guess.

The real gain, though, is another: **with an invitation, belonging is a decision
somebody makes**, not a state somebody claims. For a known circle that is the
more honest model — there is nobody who "should be able to sign up", only people
who belong.

**What this demands of the application: two procedures, not one.** Inviting and
creating a user are **not the same**, and whoever merges them notices only when
the first invitation has expired (F3):

1. **Admit somebody new** — creates the user *and* their first invitation.
2. **Create a new invitation for an existing user** — without creating a second
   user. This is the normal case after day one: expired link, lost device, one
   more device.

Without the second procedure, every re-request creates **a duplicate of the
person** — and rights, history and revocation are then spread across two records
nobody ever merges. Self-service ("request a new access link") is that same
second procedure, merely triggered by the affected person instead of the
inviter.

**Obvious and rejected.** *Username and password* — familiar, and you buy the
whole appendix with it: resets, lockout logic, requirements on the choice.
*Sign-in through a foreign provider* — spares you the safekeeping but makes
access depend on a third party and assumes everyone has an account there.
*One-time codes by mail on every visit* — solves the same problem and demands a
mailbox switch on every access; that works for rare use, not for daily.

**Symptom of violation.** There is a page where somebody can type a password —
or the same person appears twice in the list because they needed a new link.

### F3 · The invitation token is not the credential — and every device knows where it came from

**What holds.** The token in the invitation link permits **exactly one** thing:
being exchanged — and that **any number of times**. Every exchange creates
another device with its **own** credential. The credential comes into being only
then, is set by the **server**, and exists only there and in the browser.

**This is not a tolerated side effect but the purpose.** A human has a phone, a
tablet and a computer. Were the link single-use, they would need three
invitations for three devices — three mails, three procedures, three times the
question of whether it has been redeemed yet. This way **one link suffices for
one user and all their devices**, and because every exchange leaves its own
credential, each device stays individually revocable (F4). The invitation is
thus a statement about the **person** — "you belong" — not about an endpoint.

**Every device records which invitation it came from.** The invitation itself
keeps **no counter**. Together this yields the information that matters: *which*
devices came out of a link — and that can actually be acted upon.

Two procedures must exist alongside:

| Procedure | Effect |
|---|---|
| **Withdraw the invitation** | The link can no longer be redeemed. Devices already created remain **untouched** and are revoked individually |
| **Expiry of the invitation** (recommended) | The link lapses by itself, even if nobody withdraws it |

**Why the origin sits at the device and not as a number on the invitation.** A
counter says "four devices came out of this link" — and leaves you there. You
can only act once you know **which four**: only then can they be inspected,
named, revoked individually. With the origin at the device, count and last
redemption are derivable anyway; a field of its own would be a second truth that
can drift. (The trail reaches as far as the devices do: a **deleted** device
takes it along — one more reason to make revoking the rule and deleting the
exception.)

**The expiry is a recommendation, not a requirement** — but an emphatic one. A
link without expiry stays in a mailbox, and mailboxes outlive devices, people
and projects. An expiry costs nothing but the second procedure from F2: whoever
should still get in after the deadline receives a new invitation — no new
account, no duplicate. **This is precisely why the two procedures in F2 are not
a convenience but the condition under which an expiry is practicable at all.**

**Being redeemable more than once is a deliberate decision against the stricter
variant.** A single-use link would force a new procedure for every further
device of the same person — three devices, three mails. The security gain would
be small, because whoever has the link has it anyway; the real risk is the
forwarded link, and single use prevents that only the first time.

**The interplay of the three levers** — and that there are three is the point:
the **expiry** limits how long a link can work at all. **Withdrawal** ends it
early when it has fallen into the wrong hands. **Revoking individual devices**
clears up what it has already produced. None replaces another: withdrawal does
not work retroactively, revoking does not work preventively, and an expiry does
not help against the link forwarded today.

**Obvious and rejected.** *The link carries the credential* — saves the exchange
and puts the session secret permanently into a mailbox. *Single-use* — stricter,
but expensive in daily life and ineffective against the actual case. *Keeping a
counter on the invitation instead of the origin at the device* — answers "how
many", not "which"; you then know something is wrong but can do nothing.
*Doing without the expiry, "the invitation does get used"* — it gets used once
and then stays valid for years in a mailbox.

**Symptom of violation.** A session secret can be copied out of a mail — or you
see that more devices came out of an invitation than expected and cannot say
which they are.

### F4 · One credential per device — the record is "user on device"

**What holds.** Every device gets its own credential. The record behind it
describes neither a person nor a device but the connection: **this user on this
device**. The person carries name, address and rights; the device carries the
credential and its label.

**Why.** What you cannot tell apart you cannot block individually — and blocking
individually is the only procedure that counts when a device is lost. A shared
credential across all of a person's devices forces you, in an emergency, to lock
out the person instead of the device.

The separation is also the only one that models daily life correctly: a person
has several devices, and a device is used by several people. Only "user on
device" carries both. It incidentally enables information that would otherwise
be missing: the history can record **who** did something **and from which
device** — the decisive question with a lost device.

**Where the credential lives: in a cookie the server sets.** Not in storage the
application manages itself — and that has **two** reasons, of which usually only
the first is named:

1. **The client cannot read it** (`HttpOnly`). A flaw in the browser code cannot
   reach it, and it does not end up in client-side logging by accident.
2. **It survives.** Modern browsers clear storage created by **script** after a
   few days without use — tracking protection makes no distinction between
   tracking and signing in. A cookie the server sets via a response header is
   exempt and keeps its lifetime. Whoever puts the credential into script
   storage builds a model that locks everyone out after a week of holiday.

Three values and one trap you must know:

| | |
|---|---|
| Lifetime | long, but **capped** — browsers limit it to roughly 400 days, whatever value is sent. After a good year a fresh redemption is needed anyway |
| Renewal | on every visit, so a daily-used device never falls out mid-use |
| Attributes | `Secure`, and a forwarding rule that withholds the credential on foreign-initiated calls |
| **Trap** | If your address points via an alias at a **foreign** host, some browsers treat even server-set cookies like script storage — and cap them to a few days. Longevity then hangs on name resolution, not only on code |

**"Device" really means "browser profile" — and that must be said out loud.** A
credential lives in a cookie store, and a machine has several: whoever opens the
same link in two browsers gets **two** records for **one** piece of hardware. The
same goes for separate profiles, private windows and work profiles. This is not
a fault of the model — it counts accesses, not hardware — but it surprises
everyone seeing the list for the first time.

**Hence the recommendation: keep the browser's full identification string**,
which it sends on every call anyway, along with the last network address seen —
**the latest value only, no history**. It is **not a credential** and not an
identity; it is a recognition aid for the human sitting in front of the list who
must decide which of the two entries called "office computer" was the Firefox
profile. Without it the same name stands there twice and nobody dares to revoke.

**The label is assigned by the user at redemption**, not by the inviter. They
know better which device they are holding; a label guessed by an administrator
is worthless after the second device. It remains **authoritative** — the
identification string helps with recognition but decides nothing.

**Obvious and rejected.** *One credential per person* — simpler, and it makes
blocking a device impossible. *One credential per device with no person behind
it* — then nobody has an address through which to obtain a new one, and the same
person appears repeatedly with no discernible connection. *Recognizing devices
automatically* instead of naming them — technical characteristics are unreliable
and change with every update; they serve as a memory aid, not as an identifier.
*Keeping the credential in script storage* — looks tidier, forfeits `HttpOnly`
**and** longevity. *Merging several browsers of the same machine* — there is no
reliable characteristic for it, and the attempt confuses accesses with hardware;
exactly the distinction that counts when revoking.

**Symptom of violation.** A lost device can only be blocked by locking somebody
out.

### F5 · Authorization runs against the store, not against the credential

**What holds.** The credential proves **who**, not **what somebody may do**.
Rights sit with the user and are looked up on every access; they are not written
into the credential.

**What is in the credential: nothing but a reference.** It is a random value
without meaning — no role, no name, no bundle of rights, no built-in expiry. It
is stored only as a **hash**: whoever captures the store gets no working
credentials.

**What happens on every access** — four steps, and their order is part of the
decision:

1. **Resolve the credential** — hash it, find the device. Nothing found: not
   signed in.
2. **Check the device** — if it is revoked, it ends here, whoever stands behind
   it.
3. **Check the user** — if they are deactivated, it ends here, and for **all**
   their devices at once.
4. **Check the right** — only now, and **before** resolving any domain
   identifier. Otherwise the error message alone reveals whether an object
   exists.

**Why.** A right written into the credential holds until the credential expires.
With credentials valid for months — the normal case here — that means a
withdrawal takes effect months later. Looked-up rights take effect on the **next
access**, in both directions: withdrawal **and** granting, on all of the
person's devices, without anyone having to sign in again.

From the same property follow two procedures that would otherwise not exist:
revoking a **device** takes effect immediately, because step 2 finds it.
Deactivating a **user** takes effect immediately on all their devices, because
step 3 catches it there — without having to visit the devices individually.

**The credential is renewed on every visit.** Only therefore may it be
long-lived without being permanent: a device unused for a year falls out by
itself, a daily-used one never expires mid-use.

**The price is one lookup per access.** In a system that opens the store on
every access anyway it is not measurable; it would be in an architecture built
to avoid exactly that. **Whoever caches it buys back the delay they wanted to
avoid** — in exactly the amount of the caching. That is permissible but must
then be said out loud: "withdrawal takes effect within five minutes" is a
promise; "withdrawal takes effect immediately" would then be false.

**Obvious and rejected.** *Write rights into a signed credential* — saves the
lookup and is the reason revocation lists had to be invented; the revocation
list is then the lookup, only clumsier and with a second truth beside it. *Short
lifetimes instead of lookups* — the usual answer; it trades the withdrawal delay
for a renewal procedure that can itself fail, and fits badly with devices unused
for weeks. *A second credential for elevated rights* ("administration only on
the home computer") — doubles the state you must keep consistent, for protection
anyone with device access defeats anyway. *Storing the credential in clear, "it
is only a session"* — a captured store is then a collection of valid accesses.

**Symptom of violation.** After a right is withdrawn, it keeps working on one
device.

### F6 · Humans receive identity, machines receive capability

**What holds.** A human receives a credential pointing at them; what they may do
is looked up (F5). A machine receives a key permitting **one task** — backing
up, analysing — and **representing nobody**. One key per task, one per machine.

**Why.** The difference is not cosmetic. An identity credential asks "who are
you?" and looks up afterwards; a capability key carries its permission within
and asks nobody. That matches what the two actually are: a human changes
devices, gains and loses rights, has an address at which they can be reached. A
background process has none of that — it has exactly one task and runs until
somebody switches it off.

Give a machine a user account and an **identity that belongs to nobody** comes
into being: unlimited in time, without an address, without anyone who would miss
it if it were misused — and with all the rights it was given for convenience.
Conversely, a capability key is wrong for a human, because you cannot tell who
used it.

**The capability side in its strict form means: one key, one capability, one
place.** No parameter with which the caller chooses their scope — the key **is**
the scope. Whoever has two tasks gets two keys; whoever works at two places
likewise. There is then no authorization code that could be wrong, and a
revocation hits exactly one capability at exactly one place. How such a key is
to be handled beyond that — handover, storage, rotation, expiry — is a subject
of its own; section 10 names it.

**Separated per task and per machine**, because otherwise the incident costs
everything: the backup access hands out the **entire** dataset at once; the
analysis access sees only what it is currently processing. Merging those two into
one key saves a line of configuration and costs, in an incident, the difference
between them. And the key's name carries its machine, so a compromised computer
can be shut down **individually** — the same argument as in F4.

**Obvious and rejected.** *A technical user for the service* — the most common
route, and it produces exactly the ownerless identity above. *One key for all
machine tasks* — saves configuration and costs everything in an incident.
*Humans via capability keys too* — then it is no longer determinable who did
something, and the history becomes worthless.

**Symptom of violation.** The user list contains an entry belonging to nobody —
or a single key opens both the analysis and the entire dataset.

### F7 · Machine accesses come into being only in the bootstrap window

**What holds.** A capability key **cannot** be created during normal operation,
not even by an administrator. It comes into being exclusively while a bootstrap
window is open (F8). Its clear text appears **exactly once**; afterwards the
system holds only a hash.

**The window opens at every deployment, not only the first.** This makes the
decision no restriction in daily life but a scheduling rule: whoever connects a
new machine deploys and has its keys created in the same move — the person
setting up holds the setup secret anyway while deploying. There is no case of "I
need a key now and cannot get one"; there is only "I need a key, so I deploy".

**Why.** An administrative credential sits on a device for months. If it could
issue a machine key, a single captured device would suffice to obtain a
**permanent** access — one that survives the device being blocked and, in the
worst case, hands out the entire dataset. Creating permanent machine accesses
therefore belongs behind the same threshold as the bootstrap itself.

The second reason is an attitude that pays off: **a machine access is a
deliberate act, not a by-product.** An administrator wanting to back up by hand
creates a key for it — rather than such access running along as a silent
side-right of a role.

**Creating is time-limited — revoking is always available.** This asymmetry is
deliberate and must be stated, because it does not follow by itself: a
capability key can be **shut down during normal operation**, without a window,
without a deployment, without the setup secret — an administrator suffices.
Otherwise a compromised computer would keep running until the next deployment,
and the block would come too late exactly when it is needed.

The reasoning is the same as with devices (F4): a procedure that **takes rights
away** must never be harder to reach than the one that grants them. When
granting, you may build hurdles; when revoking, every hurdle costs time in which
the damage continues. That is why every key carries the name of its machine and
its task (F6) — otherwise, in an emergency, nobody knows which of five keys to
shut down.

**Obvious and rejected.** *Leave creation in the administration area* —
convenient, and it turns a captured administrative credential into a master key
of unlimited durability. *Bind revoking to the window too, "for symmetry"* —
sounds consistent and is the mistake: a compromised computer would then keep
running until the next deployment. *Store the clear text, "so it can be looked at
again"* — it is then a permanently readable secret, and the one-time appearance
was for nothing.

**Symptom of violation.** A machine key can be displayed in clear afterwards —
or created from within the running application.

### F8 · The bootstrap is a time window, not a delete-me file

**What holds.** Procedures that may only exist during setup — creating the first
administrator, creating machine keys, migrating the schema — require **two
things**: the system's strongest secret, held outside the store, **and** a time
window opened by a deployment. Afterwards the same procedures answer that the
window is closed.

**Why.** A fresh system has nobody who could permit anything — it must be able to
unlock itself without standing open permanently. The time window solves that
without inventing a second secret: whoever can deploy has access to the
configuration anyway; they cannot obtain rights they did not already have — the
bootstrap merely makes them **conveniently reachable for a limited time**.

**Why the secret must lie outside the store:** it is needed precisely when the
store is empty, damaged or mid-migration. Were it inside, it would be
unreachable at the moment it is needed.

**How the window comes about in practice** — *example, deliberately plain:* the
deployment script writes a small file next to the configuration on every run,
containing two things: the **setup secret** and an **expiry timestamp** (say, an
hour later). The setup procedures check both; once the timestamp has passed,
they answer that the window is closed. No timer is needed, no background
process, no state in the database — only a comparison against the clock.

**And here lies the actual justification of the whole procedure:** whoever can
write that file **has access to the machine anyway**. They could replace the
application, read the store, do anything. So the bootstrap gives them no rights
they did not already have — it merely makes them **conveniently reachable for a
limited time**. That is precisely why it is no contradiction that a window
exists in which more is possible than usual: the trust boundary does not run at
the window but at write access to the machine, and it was already there.

**Obvious and rejected.** *A setup file you delete after installation* — the
classic route, and it relies on somebody remembering; it is forgotten in exactly
the cases where it would matter. *A separate setup secret* — one more place
where things can drift, with no gain over the one that already exists. *Opening
and closing the window by hand* — then it stays open.

**Symptom of violation.** There is a file whose presence decides whether
somebody can take over the system.

### F9 · Secrets in addresses sit in the fragment — or nowhere

**What holds.** If a secret must reach the client via an **address** — an
invitation token, an entry key — it sits after the `#`. Not in the path, not in
the query string. From there the application reads it and sends it **once** in
the body of a call; afterwards it appears in no address.

**Why.** Everything before the `#` the browser sends to the server, and the
server writes it into the access log. It also sits in the browser history and in
every `Referer` the browser sends to foreign sites. A secret there is not
compromised when somebody attacks, but **from the outset**, through normal
operation. The browser does **not** send the fragment — it is the only place in
an address where a secret may sit.

The credential itself then travels not in the address but as a cookie the
**server** sets. That the server sets it and not the JavaScript is no detail: a
cookie the client cannot read does not leak through a flaw in the client and
does not end up in logging by accident (F4).

**Obvious and rejected.** *Token in the query string, "it is only a link"* — the
most obvious route and the worst; it distributes the secret across three logs
before anyone has done anything wrong. *Token in the path* — the same, plus
`Referer`. *Manage the credential in the client and send it as a header* — looks
tidier and forfeits the protection against the client giving it away.

**Symptom of violation.** The access log contains a line you can get in with.

### F10 · The model provides two anchors — the application hangs its rights on them

**What holds.** This model prescribes **no rights**. It is the substructure of a
domain application, and only that application knows what an invitee may do. What
is provided are **two anchors** to which rights can be attached:

| Anchor | carries |
|---|---|
| **User** | everything that applies to the **person**: domain rights, memberships, mail address |
| **User on device** | everything that applies to the **single access**: label, origin, revocation, last use |

To these anchors the application ties its own rights system. This dossier demands
**five properties** of it — not because they are required by the domain, but
because the access model does not hold without them:

1. **The model's own procedures are themselves subject to rights.** Inviting,
   withdrawing an invitation, revoking a device, deactivating a user — each needs
   a right. Without that, every invitee may invite further, and belonging is no
   longer a decision but a chain reaction (F2).
2. **Domain rights hang on the user, not on the device.** Otherwise a right
   disappears when a device is revoked — and with three devices the pointless
   question arises which is "the one with the right".
3. **Revoking one's own device requires no special right.** That is signing out.
   It is the only procedure the affected person can trigger **themselves** when a
   device has gone missing and nobody is reachable — without it, you must find
   somebody at the worst possible moment.
4. **Revoking is never harder to reach than granting.** The same asymmetry as
   with capability keys (F7): when granting you may build hurdles, when taking
   away every hurdle costs time in which the damage continues.
5. **The last administrator cannot take the administrative right from
   themselves.** Otherwise the system locks itself out, and the only way back
   leads through the bootstrap — that is, through a deployment (F8). The rule is
   cheap, its absence is expensive.

**Why.** An access model that also prescribes rights is almost always wrong in a
domain application: what somebody may do follows from the subject matter, not
from the way they came in. Conversely the model must not leave the question
**entirely** open, because four of its own procedures — inviting, withdrawing,
revoking, deactivating — exist only through this model and would otherwise stand
without an owner. The dividing line is therefore: **the cut of the domain rights
is open, the responsibility for the model's own procedures is not.**

The five properties are not rights but **statements about any rights system**
built on top. They can be satisfied with two roles just as well as with fifty.

**Obvious and rejected.** *Ship a rights model* ("reader, writer, administrator")
— convenient, and wrong in the second domain application; you inherit roles that
do not fit the subject and build your own beside them. *Leave the question
entirely open* — then nobody owns revoking, and it gets improvised at the first
incident. *Treat inviting as a "small" right beside administering* — whoever may
invite grants access to the entire dataset; it is thus as powerful as
administering, and granting it separately demands a justification of what an
inviter may **not** do. *Rights on the device instead of the user* — looks like
fine-grained control ("administer only on the home computer") and doubles the
state you must keep consistent (F5).

**Symptom of violation.** Somebody has lost their device and must wait until
somebody else can revoke it — or a right has vanished along with a revoked
device.

**Load-bearing structure.** **F1 is the precondition of everything** — without
"no guests" the rest is a convenience rather than an access model. **F3 and F4
belong together:** the exchange exists so that each device can get a credential
of its own; without F4, F3 would be superfluous, without F3, F4 unreachable.
**F5 carries manageability** and is individually adoptable — it holds for any
access model with long-lived credentials. **F6 is the boundary between the two
worlds**, and **F7 and F8 are one component, not two rules**: F7 says where
capability keys may **not** come into being, F8 provides the place where they do.
Together they yield a practicable procedure — deploy, create while the window is
open, window closes by itself.

*For adopters:* whoever takes F7 without F8 has a prohibition without an
alternative place. The need does not disappear — the backup wants its key — so
creation slides back into normal operation, silently. That is not a fault of this
model but the way it breaks when adopted piecemeal.

**F9 is a transport matter** and independent of everything else. **F10 hangs on
F2 and F4**: it names the responsibility for the procedures those two introduce —
without F10 they exist, but nobody may carry them out. **Whoever takes only one
rule takes F6** — it prevents the mistake most commonly made in this situation.

## 5. Security considerations

| Threat | Typical course | Countermeasure |
|---|---|---|
| **Forwarded invitation link** | The link is forwarded or stays in a mailbox somebody else gains access to. It is redeemable more than once, so further devices come into being | Three levers that do not replace one another (F3): the **expiry** limits preventively, **withdrawal** ends it early, **revoking individual devices** clears up. Because every device knows its invitation, it is visible **which** devices are affected — not just how many |
| **Old link in an old mailbox** | An invitation from two years ago is found and redeemed. Nobody remembers it, nobody withdrew it | The recommended **expiry** (F3). It is the only lever that works without attention — withdrawal presupposes that somebody thinks of the link |
| **Lost device** | A phone with a valid credential goes missing. The credential is valid for months | Individual revocation, enabled by F4. The history additionally says **what** was done from that device — without F4 both would be impossible |
| **Captured administrative access made permanent** | Whoever takes over an administrator's device issues themselves a machine key. It survives the device being blocked and hands out the entire dataset | F7: machine keys come into being only in the bootstrap window. That is the reason for this decision, not a side effect |
| **Ownerless identity** | A background process gets a user account "like an employee". It has no address, no owner, no expiry — and nobody misses it | F6: machines receive capability, not identity. An account with no human behind it is caught by no audit, because it looks like all the others |
| **One key for everything** | Backup and analysis share a key, "it is the same service anyway". The analysis machine sits in the living room | Separation per task **and** per machine (F6). The backup access is the only one that hands out everything at once |
| **Secret in the access log** | An invitation token sits in the query string. It lands in the server log, the browser history and the `Referer` | F9: fragment or nowhere. This threat arises without an attack, through normal operation alone |
| **Right withdrawn, no effect** | An administrative right is withdrawn, but the credential on the device keeps working | F5: rights are looked up, not carried along |

**What the concept does not protect against:** it does not protect against a
taken-over device — whoever has it has the credential. It does not replace a
rights check in the individual procedures; it only says **who** is calling, not
**whether they may**. And it knows no second factor: possession of a set-up
device is the only credential.

## 6. What it costs

- **A forwarded link works until somebody acts.** Expiry and withdrawal work only
  forwards; what the link has already produced must be revoked individually. That
  is visible, but it is work.
- **The expiry forces a second procedure.** Whoever lets invitations lapse needs
  the function "new invitation for an existing user" (F2) — otherwise duplicates
  of the person appear. That is not an addition but the price of the expiry.
- **No access without somebody who invites.** On a fresh system the bootstrap is
  the only way in; if the bootstrap secret is lost, only access to the storage
  helps.
- **Credentials are long-lived — so their loss has consequences.** The model
  stands and falls with a lost device actually being revoked; that never happens
  automatically.
- **Two worlds instead of one.** Humans and machines are administered separately.
  That is the point, but it costs two surfaces, two lists, two routes.
- **Looking up rights costs one access more**, on every call.
- **Without a password there is nothing a human can repeat.** Whoever loses their
  device and has no way to obtain a new link themselves depends on somebody else.

## 7. Scope and limits

**Preconditions.** A circle that can be **named** — somebody knows who belongs.
Clients allowed to keep a cookie. A place that invites. A deployment that can
open a bootstrap window.

**Tipping conditions.** The model becomes wrong when:

- **everyone should be able to sign themselves up** — then belonging is no longer
  a decision, and the invitation is a facade;
- the circle grows so large that **nobody surveys** who was invited — the
  visibility from F3 substitutes for prevention only while somebody looks;
- a **second factor is required** by regulation — possession of a set-up device
  is exactly one;
- **credentials must be short-lived**, for instance on shared computers; this
  model lives on long-lived credentials and becomes an imposition with short
  ones.

**Not solved.** This dossier prescribes **no rights model** — which domain
rights exist, how finely they are cut and who gets them is for the application to
decide. What it does **not** leave open are the five requirements from F10 on any
rights system built here; in particular, responsibility for inviting and revoking
is not left to improvisation. It says nothing about the **shape of the
interface** over which credentials travel, and nothing about the **delivery of
the application** itself. And it describes no way to detect a lost device
**automatically**; that remains a human decision.

## 8. Track record

> **Example** (an inventory system; names and values invented), from the
> invitation to the machine access.
>
> **Inviting.** An administrator creates a user "Anna" with her address and
> generates an invitation. Out comes a link:
>
> ```
> https://…/#invite=k7Qx2mVd9pLw
> ```
>
> Everything after the `#` the server never sees (F9).
>
> **Redeeming.** Anna opens the link on her phone. The application reads the
> token from the fragment and sends it; the server creates a device, asks for a
> label — Anna types "Anna's phone" — and answers with a cookie it sets itself.
> From now on the invitation token is meaningless *for this device* (F3).
>
> In the evening she opens the same link on the tablet. A **second** device comes
> into being with its own credential, "kitchen tablet" (F4). Both devices record
> which invitation they came from; the administration therefore shows not "two
> redemptions" but **these two devices** (F3).
>
> **Losing.** The tablet disappears. The administrator revokes **that one**
> device; Anna's phone keeps working, and the history shows what was changed from
> the tablet (F4). On the tablet's next access the check ends at step 2 —
> immediately, without any credential having to expire (F5).
>
> **Clearing up.** Because Anna's invitation lapses after the configured deadline
> anyway, nobody has to remember to withdraw it. If Anna later needs a third
> device, the administrator creates **a new invitation for the existing user** —
> not a second "Anna" (F2).
>
> **Rights.** Anna is granted the administrative right. She need not sign in
> anywhere anew — on her next access the server looks the right up at the user and
> finds it, on both devices (F5). On withdrawal the same applies, immediately.
>
> **Machine.** A computer is to back up at night. It gets **no** account but a
> capability key, like this:
>
> The deployment script writes a file next to the configuration on its run, say
> `setup-config.php`, with two values:
>
> ```php
> <?php return [
>   'setup_token' => 'A7fK…',            // the same secret the operator holds
>   'valid_until' => '2026-08-15T19:42', // one hour after the run
> ];
> ```
>
> While the clock is below that, the setup procedures are open. In this window
> the operator has the API create **all** required machine keys — one with the
> task *backup* for the backup machine, one with the task *analyse* for the
> analysis machine, each named after its machine (F6). Each clear text appears
> **exactly once** and moves into that machine's configuration. Then the hour
> elapses and the same calls answer that the window is closed (F7, F8).
>
> **Whoever could write that file had access to the machine anyway** — the window
> gives them nothing they did not already have.
>
> If a third machine joins half a year later, that is no special case: deploy,
> create its key in the then-open window, done.
>
> **Revoking, by contrast, is always possible.** If the analysis machine is
> compromised, an administrator shuts its key down **immediately** — without a
> deployment, without a window (F7). The backup key is untouched; and because it
> carries the task *backup*, the entire dataset was never reachable from the
> analysis machine in the first place.
>
> **The mistake this model prevents:** the user list contains no entry called
> "backup service".

**Reference implementation: omitted, because none exists yet (status: Draft).**
The model is fully decided and specified; the running application it comes from
does not implement it — it predates this and had an anonymous reading stage, that
is, exactly the state F1 excludes. The statements in this dossier are therefore
**thought through, not proven**; in particular the claim in F3 that visibility
instead of prevention suffices in practice is so far an argument.

## 9. Open questions

**Q1 · How long is the right deadline?** F3 recommends an expiry but names no
number. Too short and the inviter constantly sends new links; too long and the
expiry is a reassurance without effect. It probably depends on how quickly
invitees actually react — experience is missing here.

**Q2 · What happens if the bootstrap secret is lost?** F8 makes it the system's
strongest secret and puts it outside the store. If it is lost, the way back is
access to the storage. Is that the accepted answer, or is a second, equally
protected route needed?

**Q3 · How do you recognize a device without measuring it?** F4 relies on a label
assigned by a human, because technical characteristics are unreliable. With ten
devices of the same person that helps little. Is there a recognition that does
not rest on a fingerprint of the device?

**Q4 · Should a capability key expire?** F7 makes it a deliberate act but not a
time-limited one. For capability keys in general the question is answered
elsewhere in the affirmative — expiry as an operator's duty (section 10). Here it
is sharper, because creation is bound to a deployment (F7): an expiry would force
a deployment at a moment determined by the deadline rather than by operations. Is
that a gain or a new source of error — and would the answer be a deadline
*extended* at deployment time rather than one that strikes by itself?

## 10. Sources & related work

- **HTTP State Management (Cookies)**, RFC 6265 and its successor draft — the
  provisions on `HttpOnly`, `Secure`, `SameSite` and the effective maximum
  lifetime of 400 days, where the longevity of credentials meets its limit:
  <https://www.rfc-editor.org/rfc/rfc6265.html> (retrieved 2026-08-15).
- **Fragment identifiers in addresses**, RFC 3986 section 3.5 — the provision
  that the fragment part is not transmitted to the server, on which F9 rests:
  <https://www.rfc-editor.org/rfc/rfc3986#section-3.5> (retrieved 2026-08-15).
- **OWASP Session Management Cheat Sheet**, for placing the deviations from the
  usual sign-in solution taken here:
  <https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html>
  (retrieved 2026-08-15).
- **Deepening, not a prerequisite — the capability side in detail:** the sister
  dossier `capability-tokens-for-agents`
  (<https://github.com/schlingensiepen/proposals/tree/main/capability-tokens-for-agents>)
  treats only the second of the two worlds from F6, and far more deeply: the cut
  (one token, one capability, one place), handover over a second channel, storage
  in a fixed order, rotation, expiry, and the rules a machine consumer must
  follow.

  **The boundary between the two runs like this:** there it is the capability
  side alone, with an **agent** as consumer that registers itself and receives its
  token over a second channel. Here it is the **boundary** between identity and
  capability in a system carrying both — and the machine consumer is a process
  the operator deploys; its key therefore comes into being in the deployment (F7)
  and not through self-registration. That is no contradiction but a different
  starting case: an agent turns up, a background process is put there.

- **Deepening, not a prerequisite:** how the credentials are transported —
  exclusively in headers, never in the body and never in the address — and what
  an error object looks like that distinguishes "not signed in" from "not
  permitted" is described by the sister dossier `single-endpoint-json-rpc`
  (<https://github.com/schlingensiepen/proposals/tree/main/single-endpoint-json-rpc>).
  This dossier is complete without it: it fixes **which** credentials exist and
  **how they come into being**, not over which interface they travel.
