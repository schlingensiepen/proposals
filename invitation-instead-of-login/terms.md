---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| Credential | a piece of evidence with which a caller shows who or what it is. | Nachweis | eine Angabe, mit der ein Aufrufer belegt, wer oder was er ist. |
| Identity credential | a credential pointing at a bearer (this human on this device); what it may do is looked up. | Identitätsnachweis | ein Nachweis, der auf einen Träger verweist (diesen Menschen auf diesem Gerät); was er darf, wird nachgeschlagen. |
| Capability key | a credential permitting one task at one place without representing anyone; the key itself is the scope, the store holds its hash with its task and machine, and nothing is looked up at a person. | Fähigkeitsschlüssel | ein Nachweis, der eine Aufgabe an einem Ort erlaubt, ohne jemanden darzustellen; der Schlüssel selbst ist der Zuschnitt, der Bestand führt seinen Streuwert mit Aufgabe und Maschine, an einer Person wird nichts nachgeschlagen. |
| Invitation | a procedure producing a token whose sole purpose is to be exchanged for an identity credential. | Einladung | ein Vorgang, der einen Token erzeugt, dessen einziger Zweck es ist, gegen einen Identitätsnachweis getauscht zu werden. |
| Invitation link | the address that carries the invitation token, in its fragment. | Einladungslink | die Adresse, die den Einladungstoken trägt, im Fragment. |
| Redeeming | the exchange: invitation token in, identity credential out. Once per device, any number of times per link; do not confuse the two "once": a device needs the exchange exactly once, a link permits it without limit. | Einlösen | der Tausch: Einladungstoken hinein, Identitätsnachweis heraus. Je Gerät einmal, je Link beliebig oft; die beiden „einmal" nicht verwechseln: Ein Gerät braucht den Tausch genau einmal, ein Link erlaubt ihn ohne Begrenzung. |
| User | the record of the person: name, address, rights, memberships. | Benutzer | der Datensatz der Person: Name, Adresse, Rechte, Zugehörigkeiten. |
| Device | the record an identity credential hangs on; more precisely, "user on device". It records which invitation it came from. In practice it is a browser profile, not a piece of hardware. | Gerät | der Datensatz, an dem ein Identitätsnachweis hängt; genauer: „Benutzer auf Gerät". Er hält fest, aus welcher Einladung er entstanden ist. Praktisch ein Browserprofil, kein Stück Hardware. |
| User on device | the connection of one user with one device; carries credential, label, origin, revocation and last use. | Benutzer auf Gerät | die Verbindung eines Benutzers mit einem Gerät; trägt Nachweis, Bezeichnung, Herkunft, Entzug und letzte Nutzung. |
| Label | the name of a device, assigned by the user at redemption; authoritative. | Bezeichnung | der Name eines Geräts, vom Anwender beim Einlösen vergeben; maßgeblich. |
| Recognition aid | the browser's identification string and the last network address, latest value only; helps a human tell devices apart, is not a credential. | Wiedererkennungshilfe | der Kennungsstring des Browsers und die zuletzt gesehene Netzadresse, nur der letzte Wert; hilft dem Menschen, Geräte auseinanderzuhalten, ist kein Nachweis. |
| Revoking | rendering a single credential ineffective without affecting others. | Entziehen | einen einzelnen Nachweis unwirksam machen, ohne andere zu treffen. |
| Withdrawing | making an invitation no longer redeemable; it only prevents future redemptions. | Zurückziehen | eine Einladung nicht mehr einlösbar machen; verhindert nur künftige Einlösungen. |
| Expiry | the point after which an invitation lapses by itself. | Ablauf | der Zeitpunkt, nach dem eine Einladung von selbst verfällt. |
| Deactivating | shutting a user down, which ends access on all their devices at once. | Stilllegen | einen Benutzer außer Kraft setzen, was den Zugang auf all seinen Geräten zugleich beendet. |
| Store | the records the application keeps: users, devices, keys, content. | Bestand | die Daten, die die Anwendung führt: Benutzer, Geräte, Schlüssel, Inhalte. |
| Hash | the one-way derived value under which a credential is stored, so a captured store yields no working credentials. | Streuwert | der einweg abgeleitete Wert, unter dem ein Nachweis gespeichert ist, sodass ein erbeuteter Bestand keine funktionierenden Nachweise liefert. |
| Bootstrap window | a time-limited state in which procedures are permitted that are forbidden during normal operation. | Bootstrap-Fenster | ein befristeter Zustand, in dem Vorgänge erlaubt sind, die im laufenden Betrieb verboten sind. |
| Setup secret | the system's strongest secret, held outside the store, required together with the open window. | Setup-Geheimnis | das stärkste Geheimnis des Systems, außerhalb des Bestands abgelegt, zusammen mit dem offenen Fenster verlangt. |
| Fragment | the part of an address after the `#`. The browser does not send it to the server; it appears in no access log and no `Referer`. | Fragment | der Teil einer Adresse hinter dem `#`. Der Browser schickt ihn nicht an den Server; er steht in keinem Zugriffslog und in keinem `Referer`. |
| Anchor | a record to which the application attaches its rights: the user or the user on device. | Anker | ein Datensatz, an den die Anwendung ihre Rechte knüpft: der Benutzer oder der Benutzer auf Gerät. |
| Domain rights | what an invitee may do in the application's subject matter; decided by the application, not by this concept. | Fachrechte | was ein Eingeladener in der Sache der Anwendung darf; entscheidet die Anwendung, nicht dieses Konzept. |
| Builder | anyone who builds the application. | Erbauer | wer die Anwendung baut. |
| Operator | anyone who deploys and runs the application. | Betreiber | wer die Anwendung ausrollt und betreibt. |
| rule | an entry with one answer; giving it up means no longer using the concept. | Festlegung | ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
