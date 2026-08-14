---
name: headless-imap-provider-access
description: Consult when configuring or debugging unattended IMAP access to a mailbox at a major e-mail provider — choosing the right credential type (app password, service account, OAuth flow), or diagnosing logins that fail without an obvious reason.
---

# Headless IMAP Access at Major E-Mail Providers — Agent Guide

<!-- SELF-CONTAINMENT RULE: This file is handed to agents on its own. All facts an agent
     needs at the moment of action are written out HERE — never "see section 4" or "see
     the compendium". Duplication with the compendium is deliberate; duplication is
     cheaper than coupling. Markers in parentheses (section or subject names) are
     provenance notes for maintainers only. -->

Condensed from the compendium of the same name (German original leading). This guide
carries the facts needed at the moment of action; background, survey method, and sources
live in the compendium — nothing here requires reading it.

## When to consult

- A service, daemon, or script must log in to a mailbox over IMAP without a browser and
  without interactive 2FA, and you must pick the right credential type for the provider.
- An unattended IMAP login fails and you need the provider-specific cause.
- You advise a user on what to create in their provider account (app password, service
  account, mail-program password) so an automated system can access the mailbox.

## Facts at hand

The rule: almost every provider is served by an **app password** — created once in a
browser on ANY device (a phone suffices), then used like an ordinary password over
SASL PLAIN/LOGIN with TLS. The exceptions are Microsoft (OAuth2 mandatory) and Google
Workspace (service account preferred).

| Provider | Password login (IMAP) | App password (prerequisite) | OAuth2 headless | Recommendation |
|---|---|---|---|---|
| Gmail (personal) | off since 2022-05-30 | yes; 2FA required | no viable path (device flow has no mail scopes; refresh tokens of "Testing" apps die after 7 days) | **App password** |
| Google Workspace | off since 2025-03-14 | only if admin allows; 2FA | **service account + domain-wide delegation** (JWT → XOAUTH2) | **Service account** |
| Outlook.com (personal) | off since 2024-09-16 | dead for IMAP | **device-code flow** + persisted token cache (90-day sliding window) | Device-code flow |
| Microsoft 365 / Exchange Online | off since 2022/23 | dead | **client-credentials flow** (`IMAP.AccessAsApp`) — unattended without limit | Client credentials |
| Yahoo / AOL | off | yes; no 2FA required | partners only | **App password** |
| Apple iCloud Mail | off | mandatory; 2FA required; max. 25 | none | **App password**; server `imap.mail.me.com:993` |
| GMX / WEB.DE | works without 2FA — but IMAP must be enabled first | mandatory once 2FA is on | none | Enable IMAP; 2FA + **app password** |
| T-Online / Telekom | customer-center password does NOT work | mandatory: separate "Passwort für E-Mail-Programme" | none | **Mail-program password**; server `secureimap.t-online.de:993` |
| IONOS / Strato | works (mailbox password is its own secret) | not needed | none | **Mailbox password**; servers `imap.ionos.de:993` / `imap.strato.de:993` |
| mailbox.org | works without 2FA | optional; mandatory with 2FA | none | 2FA + **application password** |
| Posteo | works | optional (via "additional mailbox protection") | none | Protection + **app password** |

For Microsoft account types, the two OAuth flows are covered in depth by the sister
guide `microsoft-imap-app-only` (same repository); the table rows above are sufficient
to pick the flow.

## Procedure

1. Identify the provider from the mailbox domain; read its row above.
2. Have the human create the recommended credential in their account settings (browser
   on any device — the server never needs one).
3. Configure the service: SASL PLAIN/LOGIN over TLS with that credential — an app
   password behaves exactly like an ordinary password. For Microsoft and Google
   Workspace, follow the OAuth path named in the table instead.
4. Verify with a real login, then set up an alert on authentication failures — it is
   the only early warning for silently revoked credentials.

## Failure symptoms and first moves

| Symptom | Likely cause | First move |
|---|---|---|
| Login fails suddenly after months (Google, Apple) | Account password was changed → ALL app passwords revoked | Create a new app password; tell the user about the connection |
| Login fails after a period of inactivity (GMX/WEB.DE) | IMAP access auto-disabled after non-use | Re-enable in webmail (E-Mail-Einstellungen → POP3/IMAP) |
| Correct account password rejected (Telekom) | Customer-center password is not valid for mail programs | Create the separate mail-program password |
| Login never worked (GMX/WEB.DE) | IMAP was never enabled | Enable it in webmail |
| OAuth self-build breaks after 7 days (Gmail) | App in "Testing" status → refresh tokens expire | Not a viable path; use an app password |
| No app-password option (Google) | Advanced Protection or security-key-only 2FA | This account type is closed to the app-password path |

## Freshness

Facts surveyed 2026-08 (retrievals 2026-08-11), from official provider documentation,
not verified by live logins at every provider. Re-verify against primary sources as soon
as a provider announces an authentication change, a documented path fails in operation —
or at the latest six months after the survey date. Authentication policy is the fastest-
aging kind of provider fact.

## Source

This guide condenses the compendium "Headless IMAP-Zugang bei gängigen E-Mail-Providern"
(same directory: PROPOSAL.de.md leading, PROPOSAL.en.md English). On contradiction
between this guide and observed behavior, trust the observation, then the compendium's
sources — and report the drift so the compendium is re-surveyed.
