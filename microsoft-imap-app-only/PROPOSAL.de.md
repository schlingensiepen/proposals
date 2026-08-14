---
type: Kompendium
status: Entwurf
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: microsoft-imap-app-only
language: de (führend)
question: "Wie kommt ein headless System (Server, Daemon, Skript) 2026 noch per IMAP an Outlook.com- und Microsoft-365-Postfächer?"
thesis: "Basic Auth ist vollständig abgeschaltet und App-Passwörter sind für Mail funktionslos — es bleibt OAuth2 mit SASL XOAUTH2; für beide Kontotypen existiert ein browserloser Weg: der Client-Credentials-Flow für Microsoft 365 (unbegrenzt unbeaufsichtigt) und der Device-Code-Flow mit persistiertem Token-Cache für Privatkonten (90-Tage-Fenster)."
surveyed: 2026-08
---

# IMAP-Zugriff auf Microsoft-Postfächer ohne Browser

**In einem Satz:** Für Microsoft-Postfächer führt kein Weg an OAuth2 vorbei — aber beide Kontotypen haben einen browserlosen: Microsoft 365 über den Client-Credentials-Flow (die App meldet sich selbst an, nie ein Browser, unbegrenzt unbeaufsichtigt), private Outlook.com-Konten über den Device-Code-Flow mit persistiertem Token-Cache (einmalige Bestätigung auf einem beliebigen Gerät, danach unbeaufsichtigt im 90-Tage-Fenster).

**Kurzthese:** Wer für ein Microsoft-Postfach noch „App-Passwort erzeugen" empfiehlt, empfiehlt etwas Abgeschaltetes. Dieses Kompendium liefert für beide Kontotypen den vollständigen Einrichtungsweg — inklusive des Schritts, den die meisten Anleitungen verschweigen und ohne den der Login trotz gültigem Token scheitert —, lauffähigen Referenzcode und die Fehlerbilder-Tabelle für den Betrieb.

## 1. Problem

Ein headless System — ein Server, ein Daemon, ein Skript — soll dauerhaft per IMAP auf ein Microsoft-Postfach zugreifen: Mails ablegen, lesen, sortieren. Die klassische Antwort, Benutzername und Passwort (Basic Auth), hat Microsoft endgültig abgeschaltet; und anders als bei fast allen anderen Providern helfen auch **App-Passwörter nicht mehr** — sie sind technisch Basic Auth. Wer die einschlägigen Anleitungen im Netz befolgt, richtet deshalb etwas Funktionsloses ein. Die Frage dieses Kompendiums: Welche Wege funktionieren heute wirklich, wie richtet man sie vollständig ein, und woran scheitern sie im Betrieb?

## 2. Überblick

Es führt kein Weg an OAuth 2.0 mit dem SASL-Mechanismus `XOAUTH2` vorbei. Die gute Nachricht: Für beide Kontotypen existiert ein browserloser Weg, und einer davon ist für den Dauerbetrieb sogar besser als das alte Passwort-Modell.

```text
Welcher Kontotyp?
│
├── Microsoft 365 / Exchange Online (Organisation, eigener Tenant)
│   └── WEG A: Client-Credentials-Flow ("app-only")
│       Nie ein Browser, keine Refresh-Tokens, unbegrenzt unbeaufsichtigt.
│       Einmalige Einrichtung durch einen Admin (~20 Minuten).
│
└── Outlook.com / Hotmail / Live (privates Microsoft-Konto)
    └── WEG B: Device-Code-Flow + persistierter Token-Cache
        Einmalige Bestätigung im Browser EINES BELIEBIGEN Geräts (Handy reicht).
        Läuft danach unbeaufsichtigt, solange mindestens alle 90 Tage
        ein Zugriff stattfindet (Sliding Window).
```

Beide Wege enden identisch: Access Token besorgen → als XOAUTH2-SASL-String beim IMAP-Login auf `outlook.office365.com:993` präsentieren. Der IMAP-Teil des eigenen Codes bleibt davon unberührt. Zur Einordnung die Zeitleiste der Abschaltungen:

| Datum | Ereignis |
|---|---|
| Okt 2022 – Anf. 2023 | **Exchange Online (M365):** Basic Auth für IMAP, POP, EWS, ActiveSync, RPS abgeschaltet — endgültig, nicht reaktivierbar, auch nicht durch den Microsoft-Support (Quelle 2) |
| 16.09.2024 | **Outlook.com (privat):** Basic Auth abgeschaltet; damit sind auch App-Passwörter für IMAP/SMTP funktionslos (Quellen 5, 8) |
| 01.03.2026 → 30.04.2026 | **SMTP AUTH** mit Basic Auth: erst Ablehnungen, seit 30.04.2026 vollständig abgeschaltet — der letzte Basic-Auth-Rest ist Geschichte (Quelle 4) |

## 3. Begriffe

**Basic Auth** — Anmeldung mit Benutzername und Passwort; bei Microsoft für Mail-Protokolle vollständig abgeschaltet.

**SASL XOAUTH2** — der IMAP-Anmeldemechanismus, bei dem statt eines Passworts ein OAuth2-Access-Token präsentiert wird.

**Entra ID** — Microsofts Identitätsdienst (früher Azure AD); dort finden App-Registrierung und Berechtigungsvergabe statt.

**App-Registrierung** — der „Bauplan" einer Anwendung im Tenant, mit Application (client) ID.

**Enterprise Application / Service Principal** — die konkrete Instanz dieser App im Tenant: eine eigene Objekt-Identität mit **eigener Object ID**. Die Verwechslung der beiden Object IDs (App-Registrierung vs. Enterprise Application) ist der Klassiker unter den Einrichtungsfehlern (§5).

**Application Permission** — ein Recht der App selbst, ohne angemeldeten Nutzer (Weg A); **Delegated Permission** — ein Recht im Namen eines angemeldeten Nutzers (Weg B).

**Client-Credentials-Flow** — die App authentifiziert sich selbst (Client-ID plus Secret oder Zertifikat) und erhält direkt ein Access Token; „app-only".

**Device-Code-Flow** — der Dienst zeigt Code und URL an, die Anmeldung passiert im Browser irgendeines Geräts; danach hält ein Refresh Token den Betrieb aufrecht.

**Sliding Window** — eine Gültigkeitsfrist, die sich bei jeder Nutzung erneuert; bei Refresh Tokens privater Microsoft-Konten: 90 Tage.

**Shared Mailbox** — ein funktionsgebundenes Exchange-Postfach ohne eigenen Benutzer; als Ziel für Daemon-Szenarien geeignet.

**Tenant** — der Verwaltungsbereich einer Organisation in Entra ID, mit eigener Directory (tenant) ID.

## 4. Einzelheiten

Je Weg derselbe Block mit denselben Feldern: **Prinzip** · **Voraussetzungen** · **Einrichtung** · **Anmeldung und Referenzcode** · **Eingrenzung** · **Betrieb** · **Quellen**.

### Weg A — Microsoft 365: Client-Credentials-Flow („app-only")

**Prinzip.** Die App authentifiziert sich **selbst** (Client-ID plus Secret oder Zertifikat) und erhält direkt ein Access Token für Exchange Online — kein Nutzer-Login, kein Browser, kein Refresh Token, das ablaufen könnte. Abgelaufene Access Tokens (~60–90 Minuten Lebensdauer) werden einfach neu geholt. Für Daemons ist das das robusteste Modell, das Microsoft anbietet.

**Voraussetzungen.** Einmalig: ein Konto mit Admin-Rechten im Tenant (für App-Registrierung und Admin Consent) und die Exchange-Online-PowerShell (`Install-Module ExchangeOnlineManagement`). Danach läuft alles ohne Admin.

**Einrichtung.** Fünf Schritte; Schritt 4 ist der, den die meisten Anleitungen verschweigen.

*Schritt 1 — App-Registrierung (Entra-Portal).* Entra Admin Center → *App registrations* → *New registration*: Name z. B. `imap-daemon`, Kontotyp **„Accounts in this organizational directory only"** (Single Tenant), Redirect URI leer lassen. Von der Overview-Seite notieren: **Application (client) ID** und **Directory (tenant) ID**.

*Schritt 2 — API-Berechtigung `IMAP.AccessAsApp`.* *API permissions* → *Add a permission* → Reiter **„APIs my organization uses"** → nach **„Office 365 Exchange Online"** suchen (nicht unter den vorgeschlagenen Microsoft-Graph-Kacheln!) → **Application permissions** → `IMAP.AccessAsApp` ankreuzen → hinzufügen → **„Grant admin consent for <Tenant>"** klicken; der grüne Haken muss erscheinen.

*Schritt 3 — Client Secret oder Zertifikat.* *Certificates & secrets* → *New client secret* → Laufzeit bewusst wählen (empfohlen 12, maximal 24 Monate — **Ablaufdatum in den Betriebskalender!**). Den **Value** sofort kopieren: Er ist nur einmal sichtbar und wird gern mit der „Secret ID" verwechselt — auch das ein klassischer Einrichtungsfehler. Für höhere Ansprüche: Zertifikat statt Secret (MSAL unterstützt beides; ein Zertifikat vermeidet kopierbare Klartext-Secrets).

*Schritt 4 — Service Principal in Exchange registrieren (PowerShell).* Exchange Online muss den Service Principal der App **explizit kennen**, sonst scheitert der IMAP-Login trotz gültigem Token:

```powershell
Connect-ExchangeOnline

# Die Object ID der ENTERPRISE APPLICATION ermitteln (nicht die der App-Registrierung!)
# Portal: Entra ID -> Enterprise applications -> imap-daemon -> Overview -> Object ID
# oder per Graph PowerShell:
#   (Get-MgServicePrincipal -Filter "appId eq '<APPLICATION_ID>'").Id

New-ServicePrincipal -AppId "<APPLICATION_ID>" -ObjectId "<ENTERPRISE_APP_OBJECT_ID>" `
    -DisplayName "EXO SP für imap-daemon"
```

**Die ObjectId-Falle im Klartext:** Im Portal existieren zwei Objekte mit je einer eigenen Object ID — die *App registration* und die *Enterprise application*. `New-ServicePrincipal` verlangt die der **Enterprise application**. Die falsche ID wird kommentarlos akzeptiert und führt später zu `AUTHENTICATE failed` — der am schwersten zu findende Fehler dieser Einrichtung, weil das Token-Holen dabei fehlerfrei funktioniert.

*Schritt 5 — Postfachrechte vergeben.*

```powershell
Add-MailboxPermission -Identity "support@firma.example" `
    -User "<SERVICE_PRINCIPAL_ID>" -AccessRights FullAccess
```

Das wiederholt man **pro benötigtem Postfach** — und nur für diese.

**Anmeldung und Referenzcode.** Token-Request: Authority `https://login.microsoftonline.com/<TENANT_ID>` (bei Client Credentials **niemals** `/common`), Scope exakt `https://outlook.office365.com/.default`. Der SASL-String, den der Server sehen will:

```text
base64( "user=" + postfach + "\x01" + "auth=Bearer " + access_token + "\x01\x01" )
```

Vollständiges, lauffähiges Referenzbeispiel (`pip install msal imapclient`):

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

Wer bei stdlib-`imaplib` bleiben will, baut den SASL-String von Hand (Base64 übernimmt `authenticate` selbst):

```python
import imaplib

def xoauth2(user: str, token: str) -> bytes:
    return f"user={user}\x01auth=Bearer {token}\x01\x01".encode()

imap = imaplib.IMAP4_SSL("outlook.office365.com", 993)
imap.authenticate("XOAUTH2", lambda _challenge: xoauth2("support@firma.example", "TOKEN"))
```

**Eingrenzung.** `Add-MailboxPermission` ist bei IMAP der alleinige und vollständige Eingrenzungsmechanismus: Die App kann ausschließlich auf Postfächer zugreifen, denen sie explizit zugewiesen wurde — genau das ist das Least-Privilege-Modell dieses Flows. Ein verbreitetes Missverständnis zur Einordnung: **`New-ApplicationAccessPolicy` ist hier weder nötig noch anwendbar** — dieses Instrument begrenzt Microsoft-**Graph**-Anwendungsberechtigungen (`Mail.Read` etc.), nicht `IMAP.AccessAsApp` (Quelle 7). Praxis-Tipp: Für Daemon-Szenarien bietet sich eine **Shared Mailbox** als Ziel an (in der Regel lizenzfrei bis 50 GB — im eigenen Tenant prüfen, Q1); im XOAUTH2-String steht dann direkt deren Adresse.

**Betrieb.** Der mit Abstand häufigste Ausfallgrund im zweiten Betriebsjahr ist das **ablaufende Client Secret** (§5). Access Tokens holt MSAL selbsttätig nach; Refresh Tokens existieren in diesem Flow nicht — es gibt nichts, was durch Nichtbenutzung verfallen könnte.

**Quellen.** Quellen 1, 2, 3, 7, 9, 10, 11, 12.

### Weg B — Outlook.com-Privatkonten: Device-Code-Flow

**Prinzip.** Für private Microsoft-Konten gibt es **keinen** Client-Credentials-Flow — es bleibt der delegierte Zugriff. Der Device-Code-Flow ist die headless-taugliche Variante: Der Dienst zeigt einen Code samt URL (`https://microsoft.com/devicelogin`) an; die Anmeldung passiert im Browser **irgendeines** Geräts. Danach besitzt der Dienst einen Refresh Token im MSAL-Cache und erneuert Access Tokens unbeaufsichtigt.

**Voraussetzungen.** Eine eigene App-Registrierung (public client, kein Secret) und einmalig ein Mensch mit Browser auf irgendeinem Gerät.

**Einrichtung.** App-Registrierung wie bei Weg A Schritt 1, aber Kontotyp **„… and personal Microsoft accounts"**; unter *Authentication* → **„Allow public client flows" = Yes** (sonst `AADSTS70002`). Kein Secret nötig. Delegierte Berechtigung: `IMAP.AccessAsUser.All` (unter „APIs my organization uses" → Office 365 Exchange Online → **Delegated**).

**Anmeldung und Referenzcode.**

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

Hinweis zu den Scope-Hostnamen, weil er zuverlässig verwirrt: Delegierte Scopes heißen `https://outlook.office.com/…`, der Client-Credentials-Scope `https://outlook.office365.com/.default` — der IMAP-Server ist in beiden Fällen `outlook.office365.com`. Das ist historisch gewachsen und korrekt so.

**Eingrenzung.** Entfällt als Admin-Mechanismus: Der Zugriff ist delegiert und reicht genau so weit wie die Rechte des angemeldeten Kontos; begrenzt wird durch die Wahl des Kontos, nicht durch Postfach-Zuweisungen.

**Betrieb.** **Die 90-Tage-Regel:** Refresh Tokens privater Microsoft-Konten leben in einem 90-Tage-Sliding-Window — jede Nutzung verlängert (Quelle 6). Ein Dienst, der regelmäßig zugreift, läuft unbegrenzt; liegt er länger brach oder ändert der Nutzer sein Passwort bzw. widerruft die App, ist eine neue Device-Code-Runde fällig. Betriebskonsequenz: den `invalid_grant`-Fall erkennen und einen klaren „Neu verbinden"-Hinweis ausgeben statt still zu scheitern.

**Quellen.** Quellen 1, 5, 6, 9, 10.

## 5. Sicherheits- und Betriebsfallen

Die Fehlerbilder-Tabelle — Symptom, Ursache, Abhilfe:

| Symptom | Ursache | Abhilfe |
|---|---|---|
| `AADSTS700016` Application not found | Authority zeigt auf falschen Tenant (oder `/common` bei Client Credentials) | Authority mit eigener Tenant-ID; App im richtigen Tenant registriert? |
| `AADSTS7000215` Invalid client secret | Secret falsch — häufig wurde die **Secret ID** statt des **Value** kopiert | Neues Secret anlegen, „Value" sofort kopieren |
| `AADSTS7000222` Expired client secret | Secret abgelaufen | Rotieren; Ablaufdatum in Monitoring/Kalender |
| `AADSTS65001` / Consent-Fehler | Admin Consent nie erteilt | Portal: „Grant admin consent", grünen Haken prüfen |
| `AADSTS500011` Resource principal not found | Scope vertippt | Exakt `https://outlook.office365.com/.default` |
| `AADSTS70002` public client | „Allow public client flows" steht auf No (Weg B) | In der App-Registrierung auf Yes |
| Token ok, aber IMAP `AUTHENTICATE failed` | **Häufigster Fall:** `New-ServicePrincipal` fehlt oder wurde mit der Object ID der App-Registrierung statt der **Enterprise Application** angelegt; oder `Add-MailboxPermission` fehlt | `Get-ServicePrincipal` prüfen, ggf. korrekt neu anlegen; `Get-MailboxPermission` prüfen |
| Token ok, `NO AUTHENTICATE` bei korrektem SP | IMAP-Protokoll am Postfach deaktiviert | `Set-CASMailbox -Identity … -ImapEnabled $true` |
| `invalid_grant` bei `acquire_token_silent` (Weg B) | Refresh Token >90 Tage ungenutzt, Passwort geändert oder App-Zugriff widerrufen | Device-Code-Flow erneut durchlaufen; Fall im Code explizit behandeln |
| Device Flow „hängt" | Nutzer hat den Code nicht eingegeben (Flow läuft ca. 15 Minuten) | `flow["message"]` sichtbar machen; Timeout behandeln |
| Login ok, `APPEND` scheitert | Ordnername/Hierarchie-Delimiter falsch oder fehlende Rechte auf den Ordner | Ordnerliste per `LIST` prüfen; bei Shared Mailboxes Rechteumfang kontrollieren |

Dazu die Betriebsregeln:

**Secret-/Zertifikatsrotation.** Client Secrets laufen ab (maximal 24 Monate) — der Ablauf ist bei diesem Setup der mit Abstand häufigste Ausfallgrund im zweiten Betriebsjahr. Ablaufdaten gehören ins Monitoring (Entra zeigt sie unter *Certificates & secrets*); Rotation heißt: neues Secret anlegen, Dienst umkonfigurieren, altes löschen — unterbrechungsfrei, weil beide parallel gültig sein können. Zertifikate sind die wartungsärmere Alternative.

**Monitoring.** Aussagekräftige Quellen: die Entra **Sign-in logs** (Filter auf die Client-ID; zeigen jeden Token-Request samt Fehlercode) und die eigenen IMAP-Fehlerlogs. Sinnvolle Alerts: Token-Beschaffung schlägt fehl (Weg A: fast immer Secret oder Consent; Weg B: fast immer 90-Tage-Ablauf) und `AUTHENTICATE failed` trotz Token (Rechte- oder Service-Principal-Problem).

**Least Privilege.** Weg A: nur die tatsächlich benötigten Postfächer per `Add-MailboxPermission` zuweisen — die App bekommt dadurch keinen tenantweiten Zugriff. Dediziertes Ziel-Postfach (Shared Mailbox) statt persönlicher Postfächer, wo möglich. Das Secret nur dem Dienst zugänglich (Dateirechte 0600 bzw. Container-Secret).

**Conditional Access und Tenant-Policies.** In strenger regulierten Tenants können CA-Richtlinien (Workload-Identity-Policies, Standort-Bedingungen) auch App-Logins blockieren — das äußert sich als Token-Fehler mit CA-Hinweis. Vorab mit dem Admin klären, ob die App-Identität von Policies erfasst wird; eine feste Egress-IP des Servers erleichtert Standort-Ausnahmen (Q3).

**Checkliste für die Admin-Übergabe (Weg A):** ① App-Registrierung angelegt (Single Tenant) ② `IMAP.AccessAsApp` als *Application* Permission plus Admin Consent (grüner Haken) ③ Secret erzeugt, Value sicher übergeben, Ablaufdatum notiert ④ `New-ServicePrincipal` mit der Object ID der **Enterprise Application** ⑤ `Add-MailboxPermission` je Ziel-Postfach ⑥ `Set-CASMailbox … -ImapEnabled $true` geprüft ⑦ Testlauf des Referenzbeispiels aus Weg A.

## 6. Geltung und Erhebung

**Erhebung.** Stand 11.08.2026; alle Angaben aus offiziellen Microsoft-Quellen (Dokumentation und Ankündigungen, §8), ergänzt um zwei als solche gekennzeichnete Praxisberichte zur Fehlersuche. Entstanden als Recherche für den Entwurf eines IMAP-Ablagedienstes. Der Referenzcode folgt der dokumentierten API von MSAL und IMAPClient; ein Live-Durchlauf gegen einen echten Tenant war nicht Teil der Erhebung (Q2).

**Geltungsgrenzen.** Betrachtet ist ausschließlich IMAP über XOAUTH2 für die zwei Microsoft-Kontotypen; nicht betrachtet: Exchange on-premises, Graph-basierte Zugriffe, POP/SMTP im Detail. Wer *ausschließlich* Microsoft-Postfächer bedient, sollte **Microsoft Graph statt IMAP** erwägen: modernere API, granularere Application Permissions — und dort (nur dort) ist `New-ApplicationAccessPolicy` das passende Eingrenzungswerkzeug (Quelle 7). Für provider-übergreifende Systeme bleibt IMAP die richtige Abstraktion — deshalb dieses Kompendium. Tenant-spezifische Policies (Conditional Access) können jeden hier beschriebenen Weg zusätzlich einschränken.

**Regenerier-Regel.** Neu zu erheben, sobald Microsoft eine Änderung an OAuth-Flows, Token-Lebensdauern oder der Exchange-Authentifizierung ankündigt, sobald im Betrieb ein `AADSTS`-Fehler auftritt, den §5 nicht kennt — oder spätestens **sechs Monate** nach dem Erhebungsstand.

## 7. Stand der Recherche und offene Punkte

Stand 11.08.2026, unverändert übernommen am 14.08.2026:

**Q1** Die Lizenzfreiheit von Shared Mailboxes („in der Regel lizenzfrei bis 50 GB") ist eine Verallgemeinerung aus der Lizenzpraxis — sie ist im konkreten Tenant zu prüfen, bevor man darauf baut.

**Q2** Der Referenzcode wurde gegen die dokumentierte API geschrieben, aber im Rahmen dieser Erhebung nicht gegen einen echten Tenant beziehungsweise ein echtes Privatkonto durchlaufen; die Praxisberichte (Quellen 11, 12) decken den Weg A unabhängig, ein eigener Vollzugstest steht aus.

**Q3** Wie Conditional-Access-Richtlinien Workload-Identities in der Breite behandeln, variiert je Tenant und ließ sich nicht allgemein erheben — vor Produktivsetzung mit dem jeweiligen Admin klären.

**Q4** Die Laufzeit des Device-Code-Fensters („Flow läuft ca. 15 Minuten") ist eine Praxisbeobachtung; ein offizieller, garantierter Wert wurde nicht gefunden.

## 8. Quellen

Alle Abrufe am 11.08.2026; Primärquellen (Microsoft-Dokumentation) zuerst, Praxisberichte als solche gekennzeichnet.

**Quelle 1 · Microsoft Learn: OAuth für IMAP/POP/SMTP** — inklusive aller PowerShell-Schritte und des SASL-Formats. https://learn.microsoft.com/en-us/exchange/client-developer/legacy-protocols/how-to-authenticate-an-imap-pop-smtp-application-by-using-oauth

**Quelle 2 · Microsoft Learn: Basic-Auth-Abschaltung in Exchange Online.** https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-basic-authentication-exchange-online

**Quelle 3 · Microsoft Tech Community: Ankündigung Client-Credentials-Flow für POP/IMAP.** https://techcommunity.microsoft.com/blog/exchange/announcing-oauth-2-0-client-credentials-flow-support-for-pop-and-imap-protocols-/3562963

**Quelle 4 · Microsoft Tech Community: SMTP-AUTH-Abschaltungs-Timeline.** https://techcommunity.microsoft.com/blog/exchange/updated-exchange-online-smtp-auth-basic-authentication-deprecation-timeline/4489835

**Quelle 5 · Microsoft Support: Outlook.com verlangt Modern Authentication.** https://support.microsoft.com/en-us/office/modern-authentication-methods-now-needed-to-continue-syncing-outlook-email-in-non-microsoft-email-apps-c5d65390-9676-4763-b41f-d7986499a90d

**Quelle 6 · Microsoft Learn: Konfigurierbare Token-Lebensdauern** — unter anderem das 90-Tage-Fenster für MSA-Refresh-Tokens. https://learn.microsoft.com/en-us/entra/identity-platform/configurable-token-lifetimes

**Quelle 7 · Microsoft Learn: New-ApplicationAccessPolicy** — der Graph-Kontext, in dem dieses Instrument gilt. https://learn.microsoft.com/en-us/powershell/module/exchangepowershell/new-applicationaccesspolicy?view=exchange-ps

**Quelle 8 · Microsoft Q&A: App-Passwörter für IMAP funktionslos.** https://learn.microsoft.com/en-us/answers/questions/4743267/app-passwords-for-imap

**Quelle 9 · MSAL Python (Bibliothek).** https://learn.microsoft.com/en-us/entra/msal/python/

**Quelle 10 · IMAPClient (Bibliothek).** https://imapclient.readthedocs.io/

**Quelle 11 · Praxisbericht — Rebex: O365 IMAP app-only, Schritt für Schritt.** https://blog.rebex.net/office365-imap-pop3-oauth-unattended

**Quelle 12 · Praxisbericht — Office 365 IT Pros: IMAP/POP für Anwendungen.** https://office365itpros.com/2022/07/04/exchange-online-imap4-pop3/

**Quelle 13 · Schwester-Kompendium dieser Reihe: [headless-imap-provider-access](../headless-imap-provider-access/PROPOSAL.de.md).** Die Einordnung aller gängigen Provider (App-Passwörter als Regelfall, Microsoft als Ausnahme). Vertiefung, kein Verständnis-Muss.
