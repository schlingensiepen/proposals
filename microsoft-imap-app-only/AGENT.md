---
name: microsoft-imap-app-only
description: Consult when setting up or debugging unattended IMAP access to Microsoft mailboxes (Microsoft 365 / Exchange Online or personal Outlook.com accounts) — choosing between client-credentials and device-code flow, running the Entra/Exchange setup, or diagnosing AADSTS and AUTHENTICATE errors.
---

# IMAP Access to Microsoft Mailboxes Without a Browser — Agent Guide

<!-- SELF-CONTAINMENT RULE: This file is handed to agents on its own. All facts an agent
     needs at the moment of action are written out HERE — never "see section 4" or "see
     the compendium". Duplication with the compendium is deliberate; duplication is
     cheaper than coupling. Markers in parentheses (section or subject names) are
     provenance notes for maintainers only. -->

Condensed from the compendium of the same name (German original leading). This guide
carries the facts needed at the moment of action; background, survey method, and sources
live in the compendium — nothing here requires reading it.

## When to consult

- A daemon, server, or script must reach an Outlook.com or Microsoft 365 mailbox over
  IMAP without a browser.
- Any instruction suggests "create an app password" for a Microsoft mailbox — that is
  outdated: basic auth is fully off, app passwords are dead for mail. OAuth2 with SASL
  XOAUTH2 is the only supported mechanism.
- A token request or IMAP login against `outlook.office365.com` fails with an AADSTS
  code or `AUTHENTICATE failed`.

## Facts at hand

```text
Which account type?
│
├── Microsoft 365 / Exchange Online (organization, own tenant)
│   └── PATH A: client-credentials flow ("app-only")
│       Never a browser, no refresh tokens, unattended without limit.
│       One-time admin setup (~20 minutes).
│
└── Outlook.com / Hotmail / Live (personal Microsoft account)
    └── PATH B: device-code flow + persisted token cache
        One-time confirmation in the browser of ANY device (a phone suffices).
        Runs unattended afterwards as long as it is used at least every
        90 days (sliding window).
```

Both paths end identically: obtain an access token → present it as the XOAUTH2 SASL
string at IMAP login on `outlook.office365.com:993`:
`base64("user=" + mailbox + "\x01" + "auth=Bearer " + token + "\x01\x01")`.

**Path A setup (one-time, admin):** ① Entra app registration, single tenant, no redirect
URI; note application (client) ID and directory (tenant) ID. ② API permission
`IMAP.AccessAsApp` — found under "APIs my organization uses" → "Office 365 Exchange
Online" (NOT under the suggested Microsoft Graph tiles) → Application permissions → grant
admin consent (green check must appear). ③ Client secret (copy the VALUE, not the secret
ID; put the expiry date into the ops calendar) or certificate. ④ Register the service
principal in Exchange — the step most guides omit, without which login fails despite a
valid token: `New-ServicePrincipal -AppId <APPLICATION_ID> -ObjectId
<ENTERPRISE_APP_OBJECT_ID>` — the Object ID of the **Enterprise application**, not of the
app registration; the wrong ID is accepted silently and surfaces only later as
`AUTHENTICATE failed`. ⑤ `Add-MailboxPermission -Identity <mailbox> -User
<SERVICE_PRINCIPAL_ID> -AccessRights FullAccess` per mailbox — this and only this is the
least-privilege boundary for IMAP (`New-ApplicationAccessPolicy` applies to Graph
permissions, not to `IMAP.AccessAsApp`). Token request: authority
`https://login.microsoftonline.com/<TENANT_ID>` (never `/common` for client credentials),
scope exactly `https://outlook.office365.com/.default`. Access tokens live ~60–90
minutes; the library re-fetches them — there is nothing that can expire from non-use.

**Path B setup:** app registration with account type "… and personal Microsoft
accounts"; Authentication → "Allow public client flows" = Yes (otherwise `AADSTS70002`);
no secret; delegated permission `IMAP.AccessAsUser.All`. Authority
`https://login.microsoftonline.com/consumers`, scope
`https://outlook.office.com/IMAP.AccessAsUser.All` — do NOT list
offline_access/openid/profile yourself (MSAL adds these reserved scopes; listing them
raises ValueError). Persist the MSAL token cache to disk (mode 0600). The refresh token
lives in a 90-day sliding window — every use extends it; after >90 days idle, a password
change, or app revocation, a new device-code round is required: detect `invalid_grant`
and tell the user to reconnect instead of failing silently. Scope hostnames differ
(`outlook.office.com` delegated vs. `outlook.office365.com/.default` app-only); the IMAP
server is `outlook.office365.com` in both cases — historically grown and correct.

## Procedure

1. Determine the account type; pick path A (organization) or B (personal).
2. Run the setup above; for path A, hand the five-step checklist to the tenant admin.
3. Obtain the token with MSAL (`acquire_token_for_client` for A;
   `acquire_token_silent`, falling back to a device flow, for B).
4. Log in with SASL XOAUTH2 on `outlook.office365.com:993`; verify with a real
   operation (e.g. APPEND), not only with the login.
5. Set up alerts: token acquisition fails (A: almost always secret or consent; B:
   almost always the 90-day expiry) and `AUTHENTICATE failed` despite a valid token
   (rights or service-principal problem).

## Failure symptoms and first moves

| Symptom | Likely cause | First move |
|---|---|---|
| `AADSTS700016` application not found | Authority points at the wrong tenant (or `/common` with client credentials) | Use the own tenant ID in the authority |
| `AADSTS7000215` invalid client secret | The secret ID was copied instead of the VALUE | Create a new secret, copy the value immediately |
| `AADSTS7000222` expired client secret | Secret expired (max. 24 months) | Rotate; put expiry into monitoring |
| `AADSTS65001` / consent error | Admin consent never granted | Grant admin consent; check the green check mark |
| `AADSTS500011` resource principal not found | Scope mistyped | Exactly `https://outlook.office365.com/.default` |
| `AADSTS70002` public client | "Allow public client flows" is No (path B) | Set it to Yes in the app registration |
| Token OK, IMAP `AUTHENTICATE failed` | Most frequent case: `New-ServicePrincipal` missing or created with the app registration's object ID instead of the Enterprise application's; or `Add-MailboxPermission` missing | Check `Get-ServicePrincipal` and `Get-MailboxPermission`; recreate correctly |
| Token OK, `NO AUTHENTICATE` with correct SP | IMAP protocol disabled on the mailbox | `Set-CASMailbox -Identity … -ImapEnabled $true` |
| `invalid_grant` on silent acquisition (path B) | Refresh token >90 days idle, password changed, or app revoked | Run the device-code flow again; handle this case explicitly in code |
| Device flow "hangs" | User never entered the code (window ~15 minutes) | Surface `flow["message"]`; handle the timeout |
| Login OK, `APPEND` fails | Wrong folder name/hierarchy delimiter or missing folder rights | Check the folder list via `LIST`; check rights on shared mailboxes |

## Freshness

Facts surveyed 2026-08 (retrievals 2026-08-11), from official Microsoft documentation;
the reference setup was written against the documented MSAL/IMAPClient API and not
live-verified against a real tenant in this survey. Re-verify against primary sources as
soon as Microsoft announces changes to OAuth flows, token lifetimes, or Exchange
authentication, as soon as an AADSTS error appears that the table above does not know —
or at the latest six months after the survey date.

## Source

This guide condenses the compendium "IMAP-Zugriff auf Microsoft-Postfächer ohne Browser"
(same directory: PROPOSAL.de.md leading, PROPOSAL.en.md English). On contradiction
between this guide and observed behavior, trust the observation, then the compendium's
sources — and report the drift so the compendium is re-surveyed.
