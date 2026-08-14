---
type: Kompendium
status: Entwurf
version: "1.0"
date: 2026-08-14
author: Jörn Schlingensiepen
slug: headless-imap-provider-access
language: de (führend)
question: "Wie greift ein Server ohne Browser und ohne interaktive 2FA dauerhaft per IMAP auf die Postfächer gängiger E-Mail-Provider zu?"
thesis: "Bei fast allen Providern ist das Problem durch App-Passwörter gelöst — einmal im Browser irgendeines Geräts erzeugt, danach gewöhnliches SASL PLAIN/LOGIN über TLS; nur Microsoft erzwingt OAuth2 (browserlose Flows existieren), und bei Google Workspace ist der Service Account der saubere Weg."
surveyed: 2026-08
---

# Headless IMAP-Zugang bei gängigen E-Mail-Providern

**In einem Satz:** Ein Server ohne Browser kommt bei fast allen gängigen Providern mit einem App-Passwort dauerhaft per IMAP an das Postfach — erzeugt wird es einmalig im Browser irgendeines Geräts, verwendet wie ein gewöhnliches Passwort; die Ausnahmen sind Microsoft (OAuth2 zwingend, browserlose Flows vorhanden) und Google Workspace (Service Account als sauberer Weg).

**Kurzthese:** Wer einen Dienst baut, der unbeaufsichtigt Postfächer liest oder befüllt, fürchtet die Zwei-Faktor-Pflicht der Provider — zu Unrecht: Die interaktive Anmeldung findet einmalig auf einem beliebigen Gerät statt, der Server selbst sieht nie einen Browser. Dieses Kompendium sagt Provider für Provider, welcher Weg funktioniert, was er voraussetzt und woran er im Betrieb scheitert.

## 1. Problem

Ein Dienst — ein Daemon, ein Skript, ein Server — soll dauerhaft und unbeaufsichtigt per IMAP auf Postfächer zugreifen: ablegen, lesen, sortieren. Die Umgebung hat keinen Browser und keinen Menschen, der bei jeder Anmeldung einen zweiten Faktor bestätigt. Wer die Login-Hilfeseiten der Provider liest, gewinnt den Eindruck, genau das sei nicht mehr vorgesehen: Überall ist von Zwei-Faktor-Authentisierung, von abgeschalteten „unsicheren Apps" und von OAuth die Rede. Die Frage dieses Kompendiums: Welcher Anmeldeweg funktioniert bei welchem Provider heute wirklich — und was muss man je Provider wissen, bevor man ihn seinen Anwendern verspricht?

## 2. Überblick

Das gefürchtete Problem „alle Provider wollen 2FA, und auf einem headless System geht das nicht" ist bei fast allen Providern **bereits gelöst** — durch **App-Passwörter**: Man erzeugt einmalig im Browser eines *beliebigen* Geräts (das Handy genügt) ein statisches Zusatz-Passwort und verwendet es danach für gewöhnliches SASL PLAIN/LOGIN über TLS. Der Server selbst braucht nie einen Browser. Nur **Microsoft** erzwingt OAuth2 — mit browserlosen Wegen (Client-Credentials- bzw. Device-Code-Flow; Vertiefung: Quelle 17). Bei **Google Workspace** ist der saubere Weg der Service Account. Alle anderen gängigen Provider akzeptieren statische Passwörter in einer der zwei Formen: normales Postfach-Passwort oder App-Passwort.

| Provider | Passwort-Login (IMAP) | App-Passwort (Voraussetzung) | OAuth2 headless | Empfehlung headless Server |
|---|---|---|---|---|
| Gmail (privat) | ❌ aus seit 30.05.2022 | ✅ 2FA Pflicht | Device Flow kann **keine** Mail-Scopes; Refresh-Token-Weg fragil | **App-Passwort** |
| Google Workspace | ❌ aus seit 14.03.2025 | (✅) wenn Admin erlaubt; 2FA | **Service Account + Domain-wide Delegation** | **Service Account** |
| Outlook.com (privat) | ❌ aus seit 16.09.2024 | ❌ für IMAP funktionslos | **Device-Code-Flow** + Token-Cache (90-Tage-Fenster) | Device-Code (Quelle 17) |
| Microsoft 365 / Exchange Online | ❌ aus 2022/23 | ❌ | **Client-Credentials-Flow** — unbegrenzt unbeaufsichtigt | Client Credentials (Quelle 17) |
| Yahoo / AOL | ❌ | ✅ ohne 2FA-Pflicht | nur geprüfte Partner | **App-Passwort** |
| Apple iCloud Mail | ❌ | ✅ Pflicht; 2FA zwingend; max. 25 | ❌ | **App-Passwort** |
| GMX / WEB.DE | ✅ ohne 2FA — IMAP erst aktivieren | ✅ Pflicht bei aktivierter 2FA | ❌ | IMAP freischalten; 2FA + **App-Passwort** |
| T-Online / Telekom | ❌ (Kundencenter-Passwort gilt nicht) | ✅ Pflicht: separates „Passwort für E-Mail-Programme" | ❌ | **Programm-Passwort** |
| IONOS / Strato | ✅ (eigenständiges Postfach-Passwort) | — | ❌ | **Postfach-Passwort** |
| mailbox.org | ✅ ohne 2FA | ✅ optional; Pflicht bei 2FA | ❌ | 2FA + **Applikationspasswort** |
| Posteo | ✅ | ✅ optional („zusätzlicher Postfachschutz") | ❌ | Postfachschutz + **App-Passwort** |

Für den Erbauer eines eigenen Dienstes folgt daraus zweierlei: Ein Client, der schlichtes SASL LOGIN/PLAIN über TLS beherrscht, bedient **alle Provider außer Microsoft** — App-Passwörter sind aus Client-Sicht gewöhnliche Passwörter. Und wer die Tür für OAuth offenhalten will, kapselt die Anmeldung hinter einer Schnittstelle, die Mechanismus und Zugangsdaten liefert (SASL XOAUTH2 als vorbereitete Ausbaustufe), statt das Passwort-Modell fest zu verdrahten.

## 3. Begriffe

**Headless** — ein System ohne Browser und ohne Menschen an der Konsole; typisch: Server, Daemon, Cron-Job.

**2FA / Zwei-Faktor-Authentisierung** — Anmeldung mit zweitem Bestätigungsschritt (App, SMS, Schlüssel); das Hindernis, um das dieses Kompendium herumführt.

**App-Passwort** — ein vom Provider erzeugtes statisches Zusatz-Passwort für genau ein Programm; umgeht die interaktive 2FA, wird beim Login wie ein gewöhnliches Passwort verwendet. Provider-Namen variieren: „anwendungsspezifisches Passwort", „Applikationspasswort", „Passwort für E-Mail-Programme".

**SASL PLAIN / LOGIN** — die klassischen IMAP-Anmeldemechanismen mit Benutzername und Passwort, über TLS übertragen.

**SASL XOAUTH2** — der Anmeldemechanismus, bei dem statt eines Passworts ein OAuth2-Access-Token präsentiert wird.

**OAuth2** — das Berechtigungsprotokoll hinter XOAUTH2; relevant sind hier drei browserlose Spielarten: **Client-Credentials-Flow** (die App meldet sich selbst an), **Device-Code-Flow** (einmalige Bestätigung auf einem beliebigen Gerät) und der **Service Account** (ein maschinelles Google-Konto, das per JWT Tokens für fremde Postfächer holt).

**Domain-wide Delegation** — die Google-Workspace-Einrichtung, mit der ein Service Account im Namen der Postfächer einer Domäne handeln darf.

**Sliding Window** — eine Gültigkeitsfrist, die sich bei jeder Nutzung erneuert; liegt der Dienst länger brach als das Fenster, ist eine neue Anmeldung fällig.

## 4. Einzelheiten

Je Provider derselbe Block mit denselben Feldern: **Passwort-Login** · **App-Passwort** · **OAuth2 headless** · **Empfehlung** · **Fallen und Hinweise** · **Quellen**.

### Gmail (privates Google-Konto)

**Passwort-Login.** Abgeschaltet seit 30.05.2022. IMAP als Protokoll ist seit 2025 dauerhaft aktiv (der Einstellungs-Schalter wurde entfernt).

**App-Passwort.** Verfügbar; Voraussetzung ist aktivierte 2-Step-Verification. 16 Zeichen, erzeugt unter der Konto-Sicherheitsverwaltung.

**OAuth2 headless.** Praktisch nicht tauglich: Googles Device Flow erlaubt keine Gmail-Scopes (offizielle Scope-Liste, Quelle 2), und selbstprovisionierte Refresh Tokens verfallen bei Apps im „Testing"-Status nach 7 Tagen (Quelle 5).

**Empfehlung.** App-Passwort.

**Fallen und Hinweise.** Ein Wechsel des Konto-Passworts **widerruft alle App-Passwörter**. Bei „Advanced Protection" oder reiner Security-Key-2FA gibt es keine App-Passwörter — solche Konten sind für diesen Weg gesperrt.

**Quellen.** Quellen 1, 2, 5.

### Google Workspace

**Passwort-Login.** Endgültig abgeschaltet seit 14.03.2025 (Abschluss des „Less secure apps"-Übergangs, Quelle 4).

**App-Passwort.** Funktioniert offiziell weiterhin (2FA plus Admin-Erlaubnis), gilt aber als Übergangslösung — vor Produktivsetzung im konkreten Tenant testen (offen: Q1).

**OAuth2 headless.** Der robuste Weg: **Service Account mit domänenweiter Delegation** — holt per JWT ohne Browser ein Access Token für das Zielpostfach (Scope `https://mail.google.com/`) und meldet sich per SASL XOAUTH2 an (Quelle 3). Kein Token-Verfall durch Nutzeraktionen.

**Empfehlung.** Service Account.

**Fallen und Hinweise.** Die Delegation wird in der Admin-Konsole der Domäne eingerichtet und wirkt domänenweit — der Schlüssel des Service Accounts ist entsprechend zu schützen wie ein Generalschlüssel für die delegierten Scopes.

**Quellen.** Quellen 3, 4, 5.

### Outlook.com / Hotmail / Live (privates Microsoft-Konto)

**Passwort-Login.** Abgeschaltet seit 16.09.2024.

**App-Passwort.** Für IMAP **funktionslos** — App-Passwörter sind technisch Basic Auth, und die ist abgeschaltet; das Feature existiert für einige Nicht-Mail-Altzwecke formal weiter, was regelmäßig Verwirrung stiftet.

**OAuth2 headless.** Device-Code-Flow mit persistiertem Token-Cache: einmalige Bestätigung im Browser eines beliebigen Geräts, danach unbeaufsichtigter Betrieb, solange mindestens alle 90 Tage ein Zugriff stattfindet (Sliding Window).

**Empfehlung.** Device-Code-Flow; Einrichtung, Referenzcode und Fehlerbilder im Schwester-Kompendium (Quelle 17).

**Fallen und Hinweise.** Liegt der Dienst länger als 90 Tage brach oder ändert der Nutzer sein Passwort, ist eine neue Device-Code-Runde fällig — den Fall im Betrieb erkennen und klar melden statt still zu scheitern.

**Quellen.** Quelle 17.

### Microsoft 365 / Exchange Online

**Passwort-Login.** Abgeschaltet 2022/23 — endgültig, nicht reaktivierbar, auch nicht durch den Microsoft-Support.

**App-Passwort.** Funktionslos (siehe Outlook.com).

**OAuth2 headless.** **Client-Credentials-Flow** („app-only", Berechtigung `IMAP.AccessAsApp`): Die App meldet sich selbst an — nie ein Browser, keine Refresh Tokens, unbegrenzt unbeaufsichtigt; einmalige Admin-Einrichtung.

**Empfehlung.** Client-Credentials-Flow; die vollständige Einrichtung samt der ObjectId-Falle steht im Schwester-Kompendium (Quelle 17).

**Fallen und Hinweise.** Der Flow verlangt einen zusätzlichen, gern verschwiegenen Registrierungsschritt in Exchange Online — ohne ihn scheitert der IMAP-Login trotz gültigem Token. Details: Quelle 17.

**Quellen.** Quelle 17.

### Yahoo / AOL

**Passwort-Login.** Abgeschaltet.

**App-Passwort.** Verfügbar **ohne 2FA-Pflicht**, erzeugt in der Account Security.

**OAuth2 headless.** Nur für geprüfte Partner nach Review — für eigene Dienste irrelevant.

**Empfehlung.** App-Passwort.

**Fallen und Hinweise.** Angenehm für Server: App-Passwörter überleben hier Änderungen des Hauptpassworts, bis man sie manuell löscht.

**Quellen.** Quellen 6, 7.

### Apple iCloud Mail

**Passwort-Login.** Abgeschaltet für Dritt-Clients.

**App-Passwort.** Pflicht („app-spezifische Passwörter"); 2FA am Apple-Konto vorausgesetzt (heute Standard); maximal 25 gleichzeitig aktive.

**OAuth2 headless.** Nicht angeboten.

**Empfehlung.** App-Passwort. Server: `imap.mail.me.com:993`.

**Fallen und Hinweise.** Wie bei Google: Ein **Passwort-Reset am Apple-Konto widerruft alle** app-spezifischen Passwörter — Monitoring einplanen. Verhalten beim 25er-Limit ist ungeprüft (Q3).

**Quellen.** Quellen 8, 9.

### GMX / WEB.DE (United Internet)

**Passwort-Login.** Möglich ohne 2FA — aber IMAP muss im Webmail **erst aktiviert** werden (E-Mail-Einstellungen → POP3/IMAP).

**App-Passwort.** Pflicht bei aktivierter 2FA („anwendungsspezifisches Passwort", unter „Login & Sicherheit").

**OAuth2 headless.** Nicht angeboten.

**Empfehlung.** IMAP freischalten; 2FA aktivieren und App-Passwort verwenden.

**Fallen und Hinweise.** Der IMAP-Zugriff wird bei längerer Nichtnutzung **automatisch wieder deaktiviert** — für Dauerdienste unkritisch, nach Betriebspausen die klassische Fehlerquelle. Die genaue Frist ist nicht dokumentiert (Q2).

**Quellen.** Quellen 10, 11.

### T-Online / Telekom

**Passwort-Login.** Das Kundencenter-Passwort gilt in Mail-Programmen grundsätzlich **nicht**.

**App-Passwort.** Pflicht: das separate **„Passwort für E-Mail-Programme"** (im E-Mail-Center unter Login-Einstellungen anzulegen; faktisch ein App-Passwort ohne 2FA-Pflicht).

**OAuth2 headless.** Nicht angeboten.

**Empfehlung.** Programm-Passwort. Server: `secureimap.t-online.de:993`.

**Fallen und Hinweise.** Der häufigste Support-Fall ist der Versuch, sich mit dem Kundencenter-Passwort anzumelden — er kann nicht funktionieren.

**Quellen.** Quelle 12.

### IONOS / Strato

**Passwort-Login.** Möglich — Postfach-Passwörter sind hier ohnehin eigenständige, vom Kundenkonto getrennte Zugangsdaten.

**App-Passwort.** Entfällt, weil das Postfach-Passwort bereits ein dediziertes Secret ist.

**OAuth2 headless.** Nicht angeboten.

**Empfehlung.** Postfach-Passwort; dediziertes Postfach je Einsatzzweck. Server: `imap.ionos.de:993` bzw. `imap.strato.de:993`.

**Fallen und Hinweise.** Die einfachsten Fälle der Tabelle: SASL PLAIN über TLS, fertig.

**Quellen.** Quellen 13, 14.

### mailbox.org

**Passwort-Login.** Möglich ohne 2FA.

**App-Passwort.** Optional („Applikationspasswort"); Pflicht bei aktivierter 2FA.

**OAuth2 headless.** Nicht angeboten.

**Empfehlung.** 2FA aktivieren und Applikationspasswort verwenden.

**Fallen und Hinweise.** Keine über das Allgemeine hinaus bekannt; Detailtiefe der Erhebung hier geringer als bei den großen Providern.

**Quellen.** Quelle 15.

### Posteo

**Passwort-Login.** Möglich.

**App-Passwort.** Optional über den „zusätzlichen Postfachschutz".

**OAuth2 headless.** Nicht angeboten.

**Empfehlung.** Postfachschutz aktivieren und App-Passwort verwenden.

**Fallen und Hinweise.** Wie mailbox.org: solide dokumentiert, aber mit geringerer Erhebungstiefe.

**Quellen.** Quelle 16.

## 5. Sicherheits- und Betriebsfallen

Der häufigste Langzeit-Ausfallgrund über alle Provider hinweg sind **widerrufene App-Passwörter nach Konto-Passwortwechseln** (Google, Apple) — ein Alert auf `AUTHENTICATIONFAILED` findet das am Tag eins statt nach Wochen. Die Fallen im Einzelnen:

| Symptom | Wahrscheinliche Ursache | Erster Handgriff |
|---|---|---|
| Login scheitert plötzlich nach Monaten (Google, Apple) | Konto-Passwort wurde geändert → alle App-Passwörter widerrufen | Neues App-Passwort erzeugen; Anwender über den Zusammenhang aufklären |
| Login scheitert nach Betriebspause (GMX/WEB.DE) | IMAP-Zugriff wurde wegen Nichtnutzung automatisch deaktiviert | Im Webmail erneut aktivieren |
| Login mit korrektem Konto-Passwort scheitert (Telekom) | Kundencenter-Passwort gilt nicht für Mail-Programme | „Passwort für E-Mail-Programme" anlegen und verwenden |
| Login scheitert von Anfang an (GMX/WEB.DE) | IMAP war nie freigeschaltet | E-Mail-Einstellungen → POP3/IMAP aktivieren |
| OAuth-Weg bricht nach 7 Tagen (Gmail, Eigenbau) | App im „Testing"-Status → Refresh Tokens verfallen | Kein tragfähiger Weg; App-Passwort verwenden |
| App-Passwort lässt sich nicht erzeugen (Google) | Advanced Protection oder reine Security-Key-2FA | Kontotyp ist für diesen Weg gesperrt; anderes Konto oder anderer Provider |

Zwei Betriebsregeln für den Dienst, der diese Provider bedient: **Ein Einrichtungsrezept je Provider** gehört in die Betriebsdokumentation — die Fallen sind banal, aber nur, wenn man sie kennt. Und **Monitoring auf Authentifizierungsfehler** ist keine Kür, sondern die einzige Früherkennung für alle oben genannten Fälle.

## 6. Geltung und Erhebung

**Erhebung.** Stand 11.08.2026, erhoben aus den offiziellen Hilfe- und Entwicklerseiten der Provider (vollständig in §8); entstanden als Recherche für den Entwurf eines IMAP-Ablagedienstes. Die Angaben wurden aus den Primärquellen übernommen, nicht durch eigene Anmeldeversuche bei jedem einzelnen Provider verifiziert — wo die Quellenlage dünner ist (mailbox.org, Posteo), steht es im jeweiligen Block.

**Geltungsgrenzen.** Betrachtet sind die gängigen Consumer- und Business-Provider im deutschsprachigen und US-Raum; keine Aussage zu Exchange on-premises, kleinen Hostern oder Behörden-Umgebungen. Betrachtet ist ausschließlich IMAP — für reine Microsoft-Umgebungen kann eine modernere API der bessere Weg sein (Quelle 17, dort §6).

**Regenerier-Regel.** Neu zu erheben, sobald ein Provider eine Änderung an Anmeldeverfahren ankündigt (wie Microsofts Basic-Auth-Abschaltung oder Googles „Less secure apps"-Übergang), sobald ein hier dokumentierter Weg im Betrieb scheitert, ohne dass §5 den Fall kennt — oder spätestens **sechs Monate** nach dem Erhebungsstand. Anmeldeverfahren sind die flüchtigste Sorte Provider-Fakten.

## 7. Stand der Recherche und offene Punkte

Stand 11.08.2026, unverändert übernommen am 14.08.2026:

**Q1** Google Workspace führt App-Passwörter offiziell als Übergangslösung — es ist offen, ob und wann sie dort abgeschaltet werden. Bis dahin gilt: vor Produktivsetzung im konkreten Tenant testen; der Service Account ist davon unabhängig.

**Q2** Die Frist, nach der GMX/WEB.DE den IMAP-Zugriff bei Nichtnutzung automatisch deaktivieren, ist in den Hilfeseiten nicht beziffert — bekannt ist nur der Mechanismus, nicht die Dauer.

**Q3** Apple begrenzt app-spezifische Passwörter auf 25 aktive; das Verhalten beim Erreichen des Limits (Fehlermeldung? stilles Verdrängen?) wurde nicht geprüft.

**Q4** mailbox.org und Posteo sind mit geringerer Tiefe erhoben als die großen Provider; Betriebsfallen jenseits der dokumentierten Grundwege können dort unentdeckt sein.

## 8. Quellen

Alle Abrufe am 11.08.2026; Primärquellen (Provider-Dokumentation) zuerst.

**Quelle 1 · Google: App-Passwörter.** https://support.google.com/accounts/answer/185833

**Quelle 2 · Google: Device Flow — erlaubte Scopes.** https://developers.google.com/identity/protocols/oauth2/limited-input-device

**Quelle 3 · Google: XOAUTH2 und Service Accounts für IMAP.** https://developers.google.com/workspace/gmail/imap/xoauth2-protocol

**Quelle 4 · Google Workspace: Übergang von „Less secure apps" zu OAuth.** https://knowledge.workspace.google.com/admin/sync/transition-from-less-secure-apps-to-oauth

**Quelle 5 · Google: OAuth2-Grundlagen und Token-Verfall.** https://developers.google.com/identity/protocols/oauth2

**Quelle 6 · Yahoo: App-Passwörter.** https://help.yahoo.com/kb/SLN15241.html

**Quelle 7 · AOL: App-Passwörter.** https://help.aol.com/articles/Create-and-manage-app-password

**Quelle 8 · Apple: App-spezifische Passwörter.** https://support.apple.com/en-us/102654

**Quelle 9 · Apple: iCloud-Mail-Serverdaten.** https://support.apple.com/en-us/102525

**Quelle 10 · GMX: POP3/IMAP aktivieren.** https://hilfe.gmx.net/email/einstellungen/pop3-imap-einschalten.html

**Quelle 11 · GMX/WEB.DE: Anwendungsspezifische Passwörter.** https://hilfe.gmx.net/sicherheit/2fa/anwendungsspezifisches-passwort.html

**Quelle 12 · Telekom: App-/Programm-Passwort für E-Mail-Programme.** https://www.telekom.de/hilfe/apps-dienste/e-mail/app-programm-passwort

**Quelle 13 · IONOS: Mail-Serverdaten (IMAP, POP3, SMTP).** https://www.ionos.com/help/email/general-topics/ionos-mail-server-details-for-imap-pop3-and-smtp/

**Quelle 14 · Strato: E-Mail-Server, Adressen und Ports.** https://www.strato.de/faq/mail/e-mailserver-adressen-ports-ssl-tls/

**Quelle 15 · mailbox.org: Applikationspasswörter für externe Programme.** https://kb.mailbox.org/de/privat/sicherheit-und-privatsphaere/applikationspasswoerter-fuer-externe-programme/

**Quelle 16 · Posteo: Zusätzlichen Postfachschutz aktivieren.** https://posteo.de/en/help/activating-additional-email-account-protection

**Quelle 17 · Schwester-Kompendium dieser Reihe: [microsoft-imap-app-only](../microsoft-imap-app-only/PROPOSAL.de.md).** Vertiefung für beide Microsoft-Kontotypen: vollständige Einrichtung des Client-Credentials- und des Device-Code-Flows samt Referenzcode, Fehlerbildern und Betriebshinweisen. Die Kernaussagen zu Microsoft stehen eigenständig in §2 und §4 dieses Kompendiums; die Vertiefung ist kein Verständnis-Muss.
