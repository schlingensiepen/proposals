---
type: Compendium
status: Draft
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: microsoft-imap-app-only
language: en — translated from the German original
sync: v1.0 (2026-08-14)
question: "How does a headless system (server, daemon, script) still reach Outlook.com and Microsoft 365 mailboxes over IMAP in 2026?"
thesis: "Basic auth is fully switched off and app passwords are dead for mail — what remains is OAuth2 with SASL XOAUTH2; a browserless path exists for both account types: the client-credentials flow for Microsoft 365 (unattended without limit) and the device-code flow with a persisted token cache for personal accounts (90-day window)."
surveyed: 2026-08
---

# IMAP Access to Microsoft Mailboxes Without a Browser

**In one sentence:** For Microsoft mailboxes there is no way around OAuth2 — but both account types have a browserless one: Microsoft 365 via the client-credentials flow (the app logs itself in, never a browser, unattended without limit), personal Outlook.com accounts via the device-code flow with a persisted token cache (a one-time confirmation on an arbitrary device, unattended thereafter within the 90-day window).

**Abstract:** Anyone who still recommends "generate an app password" for a Microsoft mailbox is recommending something that has been switched off. This compendium supplies the complete setup path for both account types — including the step that most guides pass over in silence and without which the login fails despite a valid token — plus runnable reference code and the failure-symptom table for operations.

## 1. Problem

A headless system — a server, a daemon, a script — is to access a Microsoft mailbox over IMAP permanently: filing, reading, sorting mail. Microsoft has definitively switched off the classic answer, username and password (basic auth); and unlike at almost every other provider, **app passwords no longer help** either — they are technically basic auth. Whoever follows the relevant guides on the web therefore sets up something that does not work. The question of this compendium: which paths really work today, how does one set them up completely, and where do they fail in operation?

## 2. Overview

There is no way around OAuth 2.0 with the SASL mechanism `XOAUTH2`. The good news: a browserless path exists for both account types, and one of them is even better for continuous operation than the old password model.

```text
Which account type?
│
├── Microsoft 365 / Exchange Online (organization, own tenant)
│   └── PATH A: client-credentials flow ("app-only")
│       Never a browser, no refresh tokens, unattended without limit.
│       One-time setup by an admin (~20 minutes).
│
└── Outlook.com / Hotmail / Live (personal Microsoft account)
    └── PATH B: device-code flow + persisted token cache
        One-time confirmation in the browser of ANY device (a phone suffices).
        Runs unattended afterwards as long as an access happens at least
        every 90 days (sliding window).
```

Both paths end identically: obtain an access token → present it as the XOAUTH2 SASL string at the IMAP login on `outlook.office365.com:993`. The IMAP part of your own code remains untouched by this. For context, the timeline of the shutdowns:

| Date | Event |
|---|---|
| Oct 2022 – early 2023 | **Exchange Online (M365):** basic auth switched off for IMAP, POP, EWS, ActiveSync, RPS — definitively, not reactivatable, not even by Microsoft support (Source 2) |
| 2024-09-16 | **Outlook.com (personal):** basic auth switched off; app passwords for IMAP/SMTP are thereby dead as well (Sources 5, 8) |
| 2026-03-01 → 2026-04-30 | **SMTP AUTH** with basic auth: rejections first, fully switched off since 2026-04-30 — the last remnant of basic auth is history (Source 4) |

## 3. Terms

**Basic auth** — login with username and password; at Microsoft fully switched off for mail protocols.

**SASL XOAUTH2** — the IMAP login mechanism in which an OAuth2 access token is presented instead of a password.

**Entra ID** — Microsoft's identity service (formerly Azure AD); this is where app registration and permission granting take place.

**App registration** — the "blueprint" of an application in the tenant, with an Application (client) ID.

**Enterprise Application / service principal** — the concrete instance of this app in the tenant: an object identity of its own with **an Object ID of its own**. Confusing the two Object IDs (app registration vs. Enterprise Application) is the classic among setup errors (§5).

**Application permission** — a right of the app itself, without a signed-in user (path A); **delegated permission** — a right on behalf of a signed-in user (path B).

**Client-credentials flow** — the app authenticates itself (client ID plus secret or certificate) and receives an access token directly; "app-only".

**Device-code flow** — the service displays a code and a URL, the login happens in the browser of some device or other; afterwards a refresh token keeps operation going.

**Sliding window** — a validity period that renews itself with every use; for refresh tokens of personal Microsoft accounts: 90 days.

**Shared mailbox** — a function-bound Exchange mailbox without a user of its own; suitable as a target for daemon scenarios.

**Tenant** — the administrative scope of an organization in Entra ID, with a Directory (tenant) ID of its own.

## 4. Details

The same block with the same fields for each path: **Principle** · **Prerequisites** · **Setup** · **Login and reference code** · **Scoping** · **Operations** · **Sources**.

### Path A — Microsoft 365: client-credentials flow ("app-only")

**Principle.** The app authenticates **itself** (client ID plus secret or certificate) and receives an access token for Exchange Online directly — no user login, no browser, no refresh token that could expire. Expired access tokens (~60–90 minutes of lifetime) are simply fetched anew. For daemons this is the most robust model Microsoft offers.

**Prerequisites.** Once: an account with admin rights in the tenant (for app registration and admin consent) and Exchange Online PowerShell (`Install-Module ExchangeOnlineManagement`). After that everything runs without an admin.

**Setup.** Five steps; step 4 is the one that most guides pass over in silence.

*Step 1 — app registration (Entra portal).* Entra admin center → *App registrations* → *New registration*: name e.g. `imap-daemon`, account type **"Accounts in this organizational directory only"** (single tenant), leave the redirect URI empty. Note down from the overview page: **Application (client) ID** and **Directory (tenant) ID**.

*Step 2 — API permission `IMAP.AccessAsApp`.* *API permissions* → *Add a permission* → tab **"APIs my organization uses"** → search for **"Office 365 Exchange Online"** (not among the suggested Microsoft Graph tiles!) → **Application permissions** → tick `IMAP.AccessAsApp` → add → click **"Grant admin consent for <Tenant>"**; the green check mark must appear.

*Step 3 — client secret or certificate.* *Certificates & secrets* → *New client secret* → choose the lifetime deliberately (12 months recommended, 24 months at most — **put the expiry date into the operations calendar!**). Copy the **Value** immediately: it is visible only once and is readily confused with the "Secret ID" — that too a classic setup error. For higher demands: a certificate instead of a secret (MSAL supports both; a certificate avoids copyable plaintext secrets).

*Step 4 — register the service principal in Exchange (PowerShell).* Exchange Online must **know the app's service principal explicitly**, otherwise the IMAP login fails despite a valid token:

```powershell
Connect-ExchangeOnline

# Die Object ID der ENTERPRISE APPLICATION ermitteln (nicht die der App-Registrierung!)
# Portal: Entra ID -> Enterprise applications -> imap-daemon -> Overview -> Object ID
# oder per Graph PowerShell:
#   (Get-MgServicePrincipal -Filter "appId eq '<APPLICATION_ID>'").Id

New-ServicePrincipal -AppId "<APPLICATION_ID>" -ObjectId "<ENTERPRISE_APP_OBJECT_ID>" `
    -DisplayName "EXO SP für imap-daemon"
```

**The ObjectId trap in plain words:** in the portal there are two objects, each with an Object ID of its own — the *App registration* and the *Enterprise application*. `New-ServicePrincipal` demands the one of the **Enterprise application**. The wrong ID is accepted without comment and leads later to `AUTHENTICATE failed` — the hardest error of this setup to find, because fetching the token works flawlessly all the while.

*Step 5 — grant mailbox permissions.*

```powershell
Add-MailboxPermission -Identity "support@firma.example" `
    -User "<SERVICE_PRINCIPAL_ID>" -AccessRights FullAccess
```

This is repeated **for every mailbox needed** — and only for those.

**Login and reference code.** Token request: authority `https://login.microsoftonline.com/<TENANT_ID>` (with client credentials **never** `/common`), scope exactly `https://outlook.office365.com/.default`. The SASL string the server wants to see:

```text
base64( "user=" + postfach + "\x01" + "auth=Bearer " + access_token + "\x01\x01" )
```

Complete, runnable reference example (`pip install msal imapclient`):

```python
"""M365/Exchange Online: IMAP APPEND per Client-Credentials-Flow (app-only)."""
import msal
from imapclient import IMAPClient

TENANT_ID = "00000000-0000-0000-0000-000000000000"
CLIENT_ID = "11111111-1111-1111-1111-111111111111"
CLIENT_SECRET = "…"                    # aus Env/Secret-Ablage lesen, nicht hardcoden
MAILBOX = "support@firma.example"      # Ziel-Postfach oder Shared Mailbox

_app = msal.ConfidentialClientApplication(
    client_id=CLIENT_ID,
    client_credential=CLIENT_SECRET,   # alternativ Zertifikat:
    #   {"private_key": open("key.pem").read(), "thumbprint": "…"}
    authority=f"https://login.microsoftonline.com/{TENANT_ID}",
)

def get_token() -> str:
    # MSAL cacht intern und liefert erst kurz vor Ablauf ein frisches Token.
    result = _app.acquire_token_for_client(
        scopes=["https://outlook.office365.com/.default"]
    )
    if "access_token" not in result:
        raise RuntimeError(
            f"{result.get('error')}: {result.get('error_description')}"
        )
    return result["access_token"]

def append_message(raw_message: bytes, folder: str = "INBOX") -> None:
    with IMAPClient("outlook.office365.com", ssl=True) as imap:
        imap.oauth2_login(MAILBOX, get_token())  # baut den XOAUTH2-String selbst
        imap.append(folder, raw_message)

if __name__ == "__main__":
    append_message(
        b"From: daemon@firma.example\r\n"
        b"To: support@firma.example\r\n"
        b"Subject: Ablage-Test\r\n\r\n"
        b"Hallo aus dem Daemon.\r\n"
    )
    print("APPEND ok")
```

Whoever wants to stay with the stdlib `imaplib` builds the SASL string by hand (`authenticate` takes care of the Base64 itself):

```python
import imaplib

def xoauth2(user: str, token: str) -> bytes:
    return f"user={user}\x01auth=Bearer {token}\x01\x01".encode()

imap = imaplib.IMAP4_SSL("outlook.office365.com", 993)
imap.authenticate("XOAUTH2", lambda _challenge: xoauth2("support@firma.example", "TOKEN"))
```

**Scoping.** With IMAP, `Add-MailboxPermission` is the sole and complete scoping mechanism: the app can access exclusively those mailboxes to which it has been explicitly assigned — precisely that is the least-privilege model of this flow. A widespread misconception, for context: **`New-ApplicationAccessPolicy` is neither necessary nor applicable here** — that instrument limits Microsoft **Graph** application permissions (`Mail.Read` etc.), not `IMAP.AccessAsApp` (Source 7). Practical tip: for daemon scenarios a **shared mailbox** suggests itself as the target (as a rule license-free up to 50 GB — verify in your own tenant, Q1); its address then stands directly in the XOAUTH2 string.

**Operations.** By far the most frequent cause of failure in the second year of operation is the **expiring client secret** (§5). MSAL fetches access tokens anew on its own; refresh tokens do not exist in this flow — there is nothing that could expire from non-use.

**Sources.** Sources 1, 2, 3, 7, 9, 10, 11, 12.

### Path B — personal Outlook.com accounts: device-code flow

**Principle.** For personal Microsoft accounts there is **no** client-credentials flow — delegated access is what remains. The device-code flow is the headless-capable variant: the service displays a code together with a URL (`https://microsoft.com/devicelogin`); the login happens in the browser of **any** device. Afterwards the service holds a refresh token in the MSAL cache and renews access tokens unattended.

**Prerequisites.** An app registration of your own (public client, no secret) and, once, a human with a browser on some device or other.

**Setup.** App registration as in path A step 1, but account type **"… and personal Microsoft accounts"**; under *Authentication* → **"Allow public client flows" = Yes** (otherwise `AADSTS70002`). No secret needed. Delegated permission: `IMAP.AccessAsUser.All` (under "APIs my organization uses" → Office 365 Exchange Online → **Delegated**).

**Login and reference code.**

```python
"""Outlook.com privat: IMAP per Device-Code-Flow mit persistiertem Token-Cache."""
import pathlib
import msal
from imapclient import IMAPClient

CLIENT_ID = "22222222-2222-2222-2222-222222222222"   # eigene public-client-App
AUTHORITY = "https://login.microsoftonline.com/consumers"
SCOPES = ["https://outlook.office.com/IMAP.AccessAsUser.All"]
# WICHTIG: offline_access/openid/profile NICHT selbst listen —
# MSAL ergänzt diese reservierten Scopes automatisch (sonst ValueError).
CACHE_FILE = pathlib.Path.home() / ".config" / "imap-daemon" / "msal_cache.json"
ACCOUNT = "jemand@outlook.com"

cache = msal.SerializableTokenCache()
if CACHE_FILE.exists():
    cache.deserialize(CACHE_FILE.read_text())

app = msal.PublicClientApplication(CLIENT_ID, authority=AUTHORITY, token_cache=cache)

def _save_cache() -> None:
    if cache.has_state_changed:
        CACHE_FILE.parent.mkdir(parents=True, exist_ok=True)
        CACHE_FILE.write_text(cache.serialize())
        CACHE_FILE.chmod(0o600)

def get_token() -> str:
    accounts = app.get_accounts()
    if accounts:  # Normalfall: silent aus dem Cache (refresht bei Bedarf)
        result = app.acquire_token_silent(SCOPES, account=accounts[0])
        if result and "access_token" in result:
            _save_cache()
            return result["access_token"]
    # Ersteinrichtung oder Refresh Token abgelaufen (>90 Tage ungenutzt):
    flow = app.initiate_device_flow(scopes=SCOPES)
    if "user_code" not in flow:
        raise RuntimeError(f"Device-Flow-Start fehlgeschlagen: {flow}")
    print(flow["message"])  # "To sign in, use a web browser to open …"
    result = app.acquire_token_by_device_flow(flow)  # blockiert bis Bestätigung
    if "access_token" not in result:
        raise RuntimeError(f"{result.get('error')}: {result.get('error_description')}")
    _save_cache()
    return result["access_token"]

with IMAPClient("outlook.office365.com", ssl=True) as imap:
    imap.oauth2_login(ACCOUNT, get_token())
    imap.append("Drafts", b"From: ich\r\nSubject: Entwurf\r\n\r\nText\r\n")
```

A note on the scope hostnames, because it reliably causes confusion: delegated scopes are named `https://outlook.office.com/…`, the client-credentials scope `https://outlook.office365.com/.default` — the IMAP server is `outlook.office365.com` in both cases. This has grown historically and is correct as it is.

**Scoping.** Not applicable as an admin mechanism: access is delegated and reaches exactly as far as the rights of the signed-in account; it is bounded by the choice of account, not by mailbox assignments.

**Operations.** **The 90-day rule:** refresh tokens of personal Microsoft accounts live in a 90-day sliding window — every use extends it (Source 6). A service that accesses the mailbox regularly runs indefinitely; if it lies idle longer, or the user changes their password or revokes the app, a new device-code round is due. Operational consequence: detect the `invalid_grant` case and emit a clear "reconnect" notice instead of failing silently.

**Sources.** Sources 1, 5, 6, 9, 10.

## 5. Security and operational pitfalls

The failure-symptom table — symptom, cause, remedy:

| Symptom | Cause | Remedy |
|---|---|---|
| `AADSTS700016` Application not found | Authority points at the wrong tenant (or `/common` with client credentials) | Authority with your own tenant ID; is the app registered in the right tenant? |
| `AADSTS7000215` Invalid client secret | Secret wrong — frequently the **Secret ID** was copied instead of the **Value** | Create a new secret, copy the "Value" immediately |
| `AADSTS7000222` Expired client secret | Secret expired | Rotate; expiry date into monitoring/calendar |
| `AADSTS65001` / consent error | Admin consent never granted | Portal: "Grant admin consent", check the green check mark |
| `AADSTS500011` Resource principal not found | Scope mistyped | Exactly `https://outlook.office365.com/.default` |
| `AADSTS70002` public client | "Allow public client flows" is set to No (path B) | Set it to Yes in the app registration |
| Token OK, but IMAP `AUTHENTICATE failed` | **Most frequent case:** `New-ServicePrincipal` is missing or was created with the Object ID of the app registration instead of that of the **Enterprise Application**; or `Add-MailboxPermission` is missing | Check `Get-ServicePrincipal`, recreate it correctly if needed; check `Get-MailboxPermission` |
| Token OK, `NO AUTHENTICATE` with a correct SP | IMAP protocol disabled on the mailbox | `Set-CASMailbox -Identity … -ImapEnabled $true` |
| `invalid_grant` on `acquire_token_silent` (path B) | Refresh token unused for >90 days, password changed, or app access revoked | Run through the device-code flow again; handle the case explicitly in code |
| Device flow "hangs" | User has not entered the code (the flow runs for about 15 minutes) | Make `flow["message"]` visible; handle the timeout |
| Login OK, `APPEND` fails | Wrong folder name/hierarchy delimiter or missing rights on the folder | Check the folder list via `LIST`; with shared mailboxes check the extent of the rights |

Plus the operating rules:

**Secret/certificate rotation.** Client secrets expire (24 months at most) — with this setup the expiry is by far the most frequent cause of failure in the second year of operation. Expiry dates belong in monitoring (Entra shows them under *Certificates & secrets*); rotation means: create a new secret, reconfigure the service, delete the old one — without interruption, because both can be valid in parallel. Certificates are the lower-maintenance alternative.

**Monitoring.** Informative sources: the Entra **sign-in logs** (filtered on the client ID; they show every token request including the error code) and your own IMAP error logs. Sensible alerts: token acquisition fails (path A: almost always the secret or consent; path B: almost always the 90-day expiry) and `AUTHENTICATE failed` despite a token (a rights or service-principal problem).

**Least privilege.** Path A: assign only the mailboxes actually needed via `Add-MailboxPermission` — this gives the app no tenant-wide access. A dedicated target mailbox (shared mailbox) instead of personal mailboxes wherever possible. Keep the secret accessible only to the service (file permissions 0600 or a container secret).

**Conditional access and tenant policies.** In more strictly regulated tenants, CA policies (workload identity policies, location conditions) can block app logins too — this manifests as a token error with a CA notice. Clarify with the admin in advance whether the app identity is covered by policies; a fixed egress IP of the server makes location exceptions easier (Q3).

**Checklist for the handover to the admin (path A):** ① app registration created (single tenant) ② `IMAP.AccessAsApp` as an *Application* permission plus admin consent (green check mark) ③ secret generated, value handed over securely, expiry date noted ④ `New-ServicePrincipal` with the Object ID of the **Enterprise Application** ⑤ `Add-MailboxPermission` per target mailbox ⑥ `Set-CASMailbox … -ImapEnabled $true` verified ⑦ test run of the reference example from path A.

## 6. Scope and survey

**Survey.** As of 2026-08-11; all statements from official Microsoft sources (documentation and announcements, §8), supplemented by two field reports on troubleshooting, marked as such. Produced as research for the design of an IMAP filing service. The reference code follows the documented API of MSAL and IMAPClient; a live run against a real tenant was not part of the survey (Q2).

**Limits of validity.** Covered is exclusively IMAP over XOAUTH2 for the two Microsoft account types; not covered: Exchange on-premises, Graph-based access, POP/SMTP in detail. Whoever serves *exclusively* Microsoft mailboxes should consider **Microsoft Graph instead of IMAP**: a more modern API, more granular application permissions — and there (and only there) `New-ApplicationAccessPolicy` is the fitting scoping tool (Source 7). For cross-provider systems IMAP remains the right abstraction — hence this compendium. Tenant-specific policies (conditional access) can restrict every path described here further.

**Regeneration rule.** To be surveyed anew as soon as Microsoft announces a change to OAuth flows, token lifetimes, or Exchange authentication, as soon as an `AADSTS` error appears in operation that §5 does not know — or at the latest **six months** after the survey date.

## 7. State of research and open points

As of 2026-08-11, carried over unchanged on 2026-08-14:

**Q1** The license-free status of shared mailboxes ("as a rule license-free up to 50 GB") is a generalization from licensing practice — it is to be verified in the specific tenant before building on it.

**Q2** The reference code was written against the documented API but, within this survey, was not run against a real tenant or a real personal account; the field reports (Sources 11, 12) cover path A independently, a full execution test of our own is outstanding.

**Q3** How conditional-access policies treat workload identities across the board varies per tenant and could not be surveyed in general terms — clarify with the respective admin before going into production.

**Q4** The runtime of the device-code window ("the flow runs for about 15 minutes") is a practical observation; no official, guaranteed value was found.

## 8. Sources

All retrievals on 2026-08-11; primary sources (Microsoft documentation) first, field reports marked as such.

**Source 1 · Microsoft Learn: OAuth for IMAP/POP/SMTP** — including all PowerShell steps and the SASL format. https://learn.microsoft.com/en-us/exchange/client-developer/legacy-protocols/how-to-authenticate-an-imap-pop-smtp-application-by-using-oauth

**Source 2 · Microsoft Learn: Basic-auth shutdown in Exchange Online.** https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-basic-authentication-exchange-online

**Source 3 · Microsoft Tech Community: Announcement of the client-credentials flow for POP/IMAP.** https://techcommunity.microsoft.com/blog/exchange/announcing-oauth-2-0-client-credentials-flow-support-for-pop-and-imap-protocols-/3562963

**Source 4 · Microsoft Tech Community: SMTP AUTH shutdown timeline.** https://techcommunity.microsoft.com/blog/exchange/updated-exchange-online-smtp-auth-basic-authentication-deprecation-timeline/4489835

**Source 5 · Microsoft Support: Outlook.com requires modern authentication.** https://support.microsoft.com/en-us/office/modern-authentication-methods-now-needed-to-continue-syncing-outlook-email-in-non-microsoft-email-apps-c5d65390-9676-4763-b41f-d7986499a90d

**Source 6 · Microsoft Learn: Configurable token lifetimes** — among other things the 90-day window for MSA refresh tokens. https://learn.microsoft.com/en-us/entra/identity-platform/configurable-token-lifetimes

**Source 7 · Microsoft Learn: New-ApplicationAccessPolicy** — the Graph context in which this instrument applies. https://learn.microsoft.com/en-us/powershell/module/exchangepowershell/new-applicationaccesspolicy?view=exchange-ps

**Source 8 · Microsoft Q&A: App passwords are dead for IMAP.** https://learn.microsoft.com/en-us/answers/questions/4743267/app-passwords-for-imap

**Source 9 · MSAL Python (library).** https://learn.microsoft.com/en-us/entra/msal/python/

**Source 10 · IMAPClient (library).** https://imapclient.readthedocs.io/

**Source 11 · Field report — Rebex: O365 IMAP app-only, step by step.** https://blog.rebex.net/office365-imap-pop3-oauth-unattended

**Source 12 · Field report — Office 365 IT Pros: IMAP/POP for applications.** https://office365itpros.com/2022/07/04/exchange-online-imap4-pop3/

**Source 13 · Sister compendium of this series: [headless-imap-provider-access](../headless-imap-provider-access/PROPOSAL.en.md).** The classification of all major providers (app passwords as the rule, Microsoft as the exception). In-depth treatment, not a prerequisite for understanding.
