---
type: Proposal Example
description: A user description of a small tool, "imap-sync", as a client would give it orally; the request that the context-free building tests of this concept ran against.
---

# Request: a small tool, call it "imap-sync"

*(Description from the user's point of view, as the client gives it orally.)*

**What it is about.** I have old mailboxes at various providers from which I want to take mails over into my main mailbox, not once but again and again, as long as the old addresses still receive mail. Concretely: what is in a particular folder of the old mailbox should also be in a particular folder of my main mailbox. What is already over there must not be copied again; I want no duplicates. And nothing is ever deleted, neither here nor there.

**How I want to use it.** I am not sitting in front of it. A script or an agent of mine triggers the sync when it suits, at night, after a holiday, whenever. Afterwards the same agent wants to be able to know whether the sync is through and what came of it: how many mails were taken over, whether something went wrong. If a provider is acting up or the connection drops, the tool should simply continue at the next attempt and not start again from zero.

**Who runs it.** I do, on my small server. I would like to set something up once for each combination, *this* source mailbox, *this* folder, *that* target mailbox, *that* folder, and then tell my script how to get at it. My script should need to know as little as possible about the credentials of my mailboxes.

**What matters to me.** It should stay small. I do not want a portal, no user management and nothing I have to update every two weeks so that it does not become insecure. If in half a year I add another old address, I want that set up in five minutes.
