---
type: Compendium
status: Draft
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: headless-imap-provider-access
language: en — translated from the German original
sync: v1.0 (2026-08-14)
question: "How does a server without a browser and without interactive 2FA reach the mailboxes of the major e-mail providers over IMAP on a permanent basis?"
thesis: "At almost every provider the problem is already solved by app passwords — created once in a browser on any device, used thereafter as an ordinary SASL PLAIN/LOGIN over TLS; only Microsoft forces OAuth2 (browserless flows exist), and at Google Workspace the service account is the clean path."
surveyed: 2026-08
---

# Headless IMAP Access at Major E-Mail Providers

**In one sentence:** At almost every major provider, a server without a browser reaches the mailbox permanently over IMAP with an app password — created once in a browser on any device, used like an ordinary password; the exceptions are Microsoft (OAuth2 mandatory, browserless flows available) and Google Workspace (service account as the clean path).

**Abstract:** Anyone building a service that reads or fills mailboxes unattended dreads the providers' two-factor requirement — wrongly so: the interactive login happens once on an arbitrary device, and the server itself never sees a browser. This compendium states, provider by provider, which path works, what it presupposes, and where it fails in operation.

## 1. Problem

A service — a daemon, a script, a server — is to access mailboxes over IMAP permanently and unattended: filing, reading, sorting. The environment has no browser and no human to confirm a second factor at every login. Reading the providers' login help pages leaves the impression that precisely this is no longer provided for: everywhere the talk is of two-factor authentication, of disabled "less secure apps", and of OAuth. The question of this compendium: which login path really works at which provider today — and what does one have to know per provider before promising it to one's users?

## 2. Overview

The dreaded problem "every provider wants 2FA, and on a headless system that cannot work" is **already solved** at almost every provider — by **app passwords**: you generate a static additional password once in the browser of *any* device (a phone suffices) and use it thereafter for an ordinary SASL PLAIN/LOGIN over TLS. The server itself never needs a browser. Only **Microsoft** forces OAuth2 — with browserless paths (client-credentials and device-code flow respectively; in depth: Source 17). At **Google Workspace** the clean path is the service account. All other major providers accept static passwords in one of two forms: an ordinary mailbox password or an app password.

| Provider | Password login (IMAP) | App password (prerequisite) | OAuth2 headless | Recommendation for a headless server |
|---|---|---|---|---|
| Gmail (personal) | ❌ off since 2022-05-30 | ✅ 2FA required | device flow supports **no** mail scopes; refresh-token path fragile | **App password** |
| Google Workspace | ❌ off since 2025-03-14 | (✅) if the admin allows it; 2FA | **service account + domain-wide delegation** | **Service account** |
| Outlook.com (personal) | ❌ off since 2024-09-16 | ❌ dead for IMAP | **device-code flow** + token cache (90-day window) | Device code (Source 17) |
| Microsoft 365 / Exchange Online | ❌ off 2022/23 | ❌ | **client-credentials flow** — unattended without limit | Client credentials (Source 17) |
| Yahoo / AOL | ❌ | ✅ no 2FA required | vetted partners only | **App password** |
| Apple iCloud Mail | ❌ | ✅ mandatory; 2FA required; max. 25 | ❌ | **App password** |
| GMX / WEB.DE | ✅ without 2FA — IMAP must be enabled first | ✅ mandatory once 2FA is on | ❌ | Enable IMAP; 2FA + **app password** |
| T-Online / Telekom | ❌ (customer-center password does not apply) | ✅ mandatory: separate "Passwort für E-Mail-Programme" (password for e-mail programs) | ❌ | **Mail-program password** |
| IONOS / Strato | ✅ (mailbox password is a credential of its own) | — | ❌ | **Mailbox password** |
| mailbox.org | ✅ without 2FA | ✅ optional; mandatory with 2FA | ❌ | 2FA + **application password** |
| Posteo | ✅ | ✅ optional ("zusätzlicher Postfachschutz", additional mailbox protection) | ❌ | Mailbox protection + **app password** |

For anyone building a service of their own, two things follow from this. A client that masters plain SASL LOGIN/PLAIN over TLS serves **every provider except Microsoft** — from the client's point of view, app passwords are ordinary passwords. And whoever wants to keep the door open for OAuth encapsulates the login behind an interface that supplies mechanism and credentials (SASL XOAUTH2 as a prepared extension stage) instead of hard-wiring the password model.

## 3. Terms

**Headless** — a system without a browser and without a human at the console; typically a server, a daemon, a cron job.

**2FA / two-factor authentication** — login with a second confirmation step (app, SMS, key); the obstacle this compendium leads around.

**App password** — a static additional password generated by the provider for exactly one program; it bypasses interactive 2FA and is used at login like an ordinary password. Provider names vary: "anwendungsspezifisches Passwort" (application-specific password), "Applikationspasswort" (application password), "Passwort für E-Mail-Programme" (password for e-mail programs).

**SASL PLAIN / LOGIN** — the classic IMAP login mechanisms with username and password, transmitted over TLS.

**SASL XOAUTH2** — the login mechanism in which an OAuth2 access token is presented instead of a password.

**OAuth2** — the authorization protocol behind XOAUTH2; three browserless variants are relevant here: the **client-credentials flow** (the app logs itself in), the **device-code flow** (a one-time confirmation on an arbitrary device), and the **service account** (a machine Google account that fetches tokens for other people's mailboxes via JWT).

**Domain-wide delegation** — the Google Workspace setup that lets a service account act on behalf of the mailboxes of a domain.

**Sliding window** — a validity period that renews itself with every use; if the service lies idle longer than the window, a fresh login is due.

## 4. Details

The same block with the same fields for every provider: **Password login** · **App password** · **OAuth2 headless** · **Recommendation** · **Pitfalls and notes** · **Sources**.

### Gmail (personal Google account)

**Password login.** Switched off since 2022-05-30. IMAP as a protocol has been permanently on since 2025 (the settings toggle was removed).

**App password.** Available; the prerequisite is activated 2-Step Verification. 16 characters, generated in the account security settings.

**OAuth2 headless.** Practically unusable: Google's device flow permits no Gmail scopes (official scope list, Source 2), and self-provisioned refresh tokens expire after 7 days for apps in "Testing" status (Source 5).

**Recommendation.** App password.

**Pitfalls and notes.** A change of the account password **revokes all app passwords**. With "Advanced Protection" or security-key-only 2FA there are no app passwords — such accounts are closed to this path.

**Sources.** Sources 1, 2, 5.

### Google Workspace

**Password login.** Definitively switched off since 2025-03-14 (completion of the "less secure apps" transition, Source 4).

**App password.** Officially still works (2FA plus admin permission), but counts as a transitional solution — test it in the specific tenant before going into production (open: Q1).

**OAuth2 headless.** The robust path: a **service account with domain-wide delegation** — it fetches an access token for the target mailbox via JWT without a browser (scope `https://mail.google.com/`) and logs in via SASL XOAUTH2 (Source 3). No token expiry caused by user actions.

**Recommendation.** Service account.

**Pitfalls and notes.** The delegation is set up in the domain's admin console and takes effect domain-wide — the service account's key must accordingly be protected like a master key for the delegated scopes.

**Sources.** Sources 3, 4, 5.

### Outlook.com / Hotmail / Live (personal Microsoft account)

**Password login.** Switched off since 2024-09-16.

**App password.** **Dead** for IMAP — app passwords are technically basic auth, and that is switched off; the feature formally lives on for a few non-mail legacy purposes, which regularly causes confusion.

**OAuth2 headless.** Device-code flow with a persisted token cache: a one-time confirmation in the browser of an arbitrary device, then unattended operation for as long as an access takes place at least every 90 days (sliding window).

**Recommendation.** Device-code flow; setup, reference code, and failure symptoms are in the sister compendium (Source 17).

**Pitfalls and notes.** If the service lies idle for more than 90 days, or the user changes their password, a new device-code round is due — detect the case in operation and report it clearly instead of failing silently.

**Sources.** Source 17.

### Microsoft 365 / Exchange Online

**Password login.** Switched off in 2022/23 — definitively, not reactivatable, not even by Microsoft support.

**App password.** Dead (see Outlook.com).

**OAuth2 headless.** **Client-credentials flow** ("app-only", permission `IMAP.AccessAsApp`): the app logs itself in — never a browser, no refresh tokens, unattended without limit; one-time admin setup.

**Recommendation.** Client-credentials flow; the complete setup including the ObjectId trap is in the sister compendium (Source 17).

**Pitfalls and notes.** The flow demands an additional registration step in Exchange Online that is readily passed over in silence — without it the IMAP login fails despite a valid token. Details: Source 17.

**Sources.** Source 17.

### Yahoo / AOL

**Password login.** Switched off.

**App password.** Available **without a 2FA requirement**, generated in Account Security.

**OAuth2 headless.** Only for vetted partners after review — irrelevant for one's own services.

**Recommendation.** App password.

**Pitfalls and notes.** Pleasant for servers: here app passwords survive changes of the main password until they are deleted manually.

**Sources.** Sources 6, 7.

### Apple iCloud Mail

**Password login.** Switched off for third-party clients.

**App password.** Mandatory ("app-specific passwords"); 2FA on the Apple account is presupposed (standard today); at most 25 active at the same time.

**OAuth2 headless.** Not offered.

**Recommendation.** App password. Server: `imap.mail.me.com:993`.

**Pitfalls and notes.** As with Google: a **password reset on the Apple account revokes all** app-specific passwords — plan for monitoring. The behavior on hitting the limit of 25 is unverified (Q3).

**Sources.** Sources 8, 9.

### GMX / WEB.DE (United Internet)

**Password login.** Possible without 2FA — but IMAP **must first be enabled** in webmail (E-Mail-Einstellungen → POP3/IMAP, the e-mail settings).

**App password.** Mandatory once 2FA is enabled ("anwendungsspezifisches Passwort", application-specific password, under "Login & Sicherheit").

**OAuth2 headless.** Not offered.

**Recommendation.** Enable IMAP; enable 2FA and use an app password.

**Pitfalls and notes.** IMAP access is **automatically disabled again** after a longer period of non-use — uncritical for permanently running services, but the classic source of error after operational pauses. The exact period is not documented (Q2).

**Sources.** Sources 10, 11.

### T-Online / Telekom

**Password login.** The customer-center password fundamentally does **not** apply in mail programs.

**App password.** Mandatory: the separate **"Passwort für E-Mail-Programme"** (password for e-mail programs; to be created in the E-Mail-Center under the login settings; in effect an app password without a 2FA requirement).

**OAuth2 headless.** Not offered.

**Recommendation.** Mail-program password. Server: `secureimap.t-online.de:993`.

**Pitfalls and notes.** The most frequent support case is the attempt to log in with the customer-center password — it cannot work.

**Sources.** Source 12.

### IONOS / Strato

**Password login.** Possible — mailbox passwords here are independent credentials anyway, separate from the customer account.

**App password.** Not applicable, because the mailbox password is already a dedicated secret.

**OAuth2 headless.** Not offered.

**Recommendation.** Mailbox password; a dedicated mailbox per purpose. Server: `imap.ionos.de:993` and `imap.strato.de:993` respectively.

**Pitfalls and notes.** The simplest cases in the table: SASL PLAIN over TLS, done.

**Sources.** Sources 13, 14.

### mailbox.org

**Password login.** Possible without 2FA.

**App password.** Optional ("Applikationspasswort", application password); mandatory once 2FA is enabled.

**OAuth2 headless.** Not offered.

**Recommendation.** Enable 2FA and use an application password.

**Pitfalls and notes.** None known beyond the general ones; the survey here is less detailed than for the large providers.

**Sources.** Source 15.

### Posteo

**Password login.** Possible.

**App password.** Optional, via the "zusätzlicher Postfachschutz" (additional mailbox protection).

**OAuth2 headless.** Not offered.

**Recommendation.** Enable mailbox protection and use an app password.

**Pitfalls and notes.** As with mailbox.org: soundly documented, but surveyed in less depth.

**Sources.** Source 16.

## 5. Security and operational pitfalls

The most frequent long-term cause of failure across all providers is **app passwords revoked after account password changes** (Google, Apple) — an alert on `AUTHENTICATIONFAILED` finds this on day one instead of after weeks. The pitfalls in detail:

| Symptom | Likely cause | First move |
|---|---|---|
| Login fails suddenly after months (Google, Apple) | The account password was changed → all app passwords revoked | Generate a new app password; explain the connection to the user |
| Login fails after an operational pause (GMX/WEB.DE) | IMAP access was automatically disabled because of non-use | Re-enable it in webmail |
| Login with the correct account password fails (Telekom) | The customer-center password does not apply to mail programs | Create and use the "Passwort für E-Mail-Programme" (password for e-mail programs) |
| Login fails from the very beginning (GMX/WEB.DE) | IMAP was never enabled | Enable it under E-Mail-Einstellungen → POP3/IMAP |
| OAuth path breaks after 7 days (Gmail, self-built) | App in "Testing" status → refresh tokens expire | Not a viable path; use an app password |
| App password cannot be generated (Google) | Advanced Protection or security-key-only 2FA | This account type is closed to this path; use another account or another provider |

Two operating rules for the service that serves these providers: **one setup recipe per provider** belongs in the operations documentation — the pitfalls are trivial, but only once you know them. And **monitoring for authentication failures** is not optional polish but the only early warning for all of the cases named above.

## 6. Scope and survey

**Survey.** As of 2026-08-11, surveyed from the providers' official help and developer pages (in full in §8); produced as research for the design of an IMAP filing service. The statements were taken from the primary sources, not verified by login attempts of our own at every individual provider — where the sources are thinner (mailbox.org, Posteo), the respective block says so.

**Limits of validity.** Covered are the major consumer and business providers in the German-speaking and US markets; no statement about Exchange on-premises, small hosting providers, or government environments. Covered is exclusively IMAP — for pure Microsoft environments a more modern API may be the better path (Source 17, §6 there).

**Regeneration rule.** To be surveyed anew as soon as a provider announces a change to login procedures (like Microsoft's basic-auth shutdown or Google's "less secure apps" transition), as soon as a path documented here fails in operation without §5 knowing the case — or at the latest **six months** after the survey date. Login procedures are the most volatile kind of provider fact.

## 7. State of research and open points

As of 2026-08-11, carried over unchanged on 2026-08-14:

**Q1** Google Workspace officially lists app passwords as a transitional solution — whether and when they will be switched off there is open. Until then: test in the specific tenant before going into production; the service account is independent of this.

**Q2** The period after which GMX/WEB.DE automatically disable IMAP access on non-use is not quantified in the help pages — only the mechanism is known, not the duration.

**Q3** Apple limits app-specific passwords to 25 active ones; the behavior on reaching the limit (error message? silent eviction?) was not verified.

**Q4** mailbox.org and Posteo were surveyed in less depth than the large providers; operational pitfalls beyond the documented basic paths may be undiscovered there.

## 8. Sources

All retrievals on 2026-08-11; primary sources (provider documentation) first.

**Source 1 · Google: App passwords.** https://support.google.com/accounts/answer/185833

**Source 2 · Google: Device flow — permitted scopes.** https://developers.google.com/identity/protocols/oauth2/limited-input-device

**Source 3 · Google: XOAUTH2 and service accounts for IMAP.** https://developers.google.com/workspace/gmail/imap/xoauth2-protocol

**Source 4 · Google Workspace: Transition from "less secure apps" to OAuth.** https://knowledge.workspace.google.com/admin/sync/transition-from-less-secure-apps-to-oauth

**Source 5 · Google: OAuth2 fundamentals and token expiry.** https://developers.google.com/identity/protocols/oauth2

**Source 6 · Yahoo: App passwords.** https://help.yahoo.com/kb/SLN15241.html

**Source 7 · AOL: App passwords.** https://help.aol.com/articles/Create-and-manage-app-password

**Source 8 · Apple: App-specific passwords.** https://support.apple.com/en-us/102654

**Source 9 · Apple: iCloud mail server settings.** https://support.apple.com/en-us/102525

**Source 10 · GMX: Enabling POP3/IMAP.** https://hilfe.gmx.net/email/einstellungen/pop3-imap-einschalten.html

**Source 11 · GMX/WEB.DE: Application-specific passwords.** https://hilfe.gmx.net/sicherheit/2fa/anwendungsspezifisches-passwort.html

**Source 12 · Telekom: App/program password for e-mail programs.** https://www.telekom.de/hilfe/apps-dienste/e-mail/app-programm-passwort

**Source 13 · IONOS: Mail server settings (IMAP, POP3, SMTP).** https://www.ionos.com/help/email/general-topics/ionos-mail-server-details-for-imap-pop3-and-smtp/

**Source 14 · Strato: E-mail servers, addresses, and ports.** https://www.strato.de/faq/mail/e-mailserver-adressen-ports-ssl-tls/

**Source 15 · mailbox.org: Application passwords for external programs.** https://kb.mailbox.org/de/privat/sicherheit-und-privatsphaere/applikationspasswoerter-fuer-externe-programme/

**Source 16 · Posteo: Activating additional mailbox protection.** https://posteo.de/en/help/activating-additional-email-account-protection

**Source 17 · Sister compendium of this series: [microsoft-imap-app-only](../microsoft-imap-app-only/PROPOSAL.en.md).** In-depth treatment for both Microsoft account types: the complete setup of the client-credentials and the device-code flow including reference code, failure symptoms, and operational notes. The core statements about Microsoft stand on their own in §2 and §4 of this compendium; the in-depth treatment is not a prerequisite for understanding.
