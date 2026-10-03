---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| start secret | A secret a service, a script or an agent needs before it can do anything; the subject of this concept. Not: secrets that arise in operation. | Start-Geheimnis | Ein Geheimnis, das ein Dienst, ein Skript oder ein Agent braucht, bevor er etwas tun kann; Gegenstand dieses Konzepts. Nicht: Geheimnisse, die im Betrieb entstehen. |
| source | A place a secret is obtained from: environment, pass-through file, once-only channel, secret manager. | Quelle | Ein Ort, von dem ein Geheimnis bezogen wird: Umgebung, Durchleitungsdatei, Einmal-Kanal, Geheimnisverwalter. |
| rank | A source's position in the ranking; the higher rank wins. | Rang | Die Position einer Quelle in der Rangfolge; der höhere Rang gewinnt. |
| ranking | The fixed order of sources: environment, pass-through file, once-only channel. | Rangfolge | Die feste Reihenfolge der Quellen: Umgebung, Durchleitungsdatei, Einmal-Kanal. |
| canonical name | The environment variable name the service proposes for a secret. | kanonischer Name | Der Umgebungsvariablenname, den der Dienst für ein Geheimnis vorschlägt. |
| pass-through file | A file with tight permissions whose content flows straight into the call and appears nowhere. | Durchleitungsdatei | Eine Datei mit engen Rechten, deren Inhalt direkt in den Aufruf fließt und nirgends erscheint. |
| once-only channel | The source used exactly once: the service's log at creation, the agent's context window when obtaining. | Einmal-Kanal | Die Quelle, die genau einmal benutzt wird: das Protokoll des Dienstes bei Erzeugung, das Kontextfenster des Agenten beim Beziehen. |
| latent access | A valid secret at a lower rank that nobody owns any more and that applies again as soon as the higher rank is missing. | latenter Zugang | Ein gültiges Geheimnis auf einem unteren Rang, das niemand mehr besitzt und das wieder wirkt, sobald der obere Rang fehlt. |
| root | A secret that performs no function but can issue and revoke accesses; its holder can finish setting up the system. | Wurzel | Ein Geheimnis, das keine Funktion ausführt, aber Zugänge ausstellen und entziehen kann; der Träger kann damit das System fertig aufsetzen. |
| inherited secret | A secret issued by a target system (API key, password); the service cannot generate it. | ererbtes Geheimnis | Ein Geheimnis, das ein Zielsystem ausgestellt hat (API-Schlüssel, Passwort); der Dienst kann es nicht erzeugen. |
| rule | An entry with one answer; giving it up means no longer using the concept. | Festlegung | Ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
| choice | An entry where the concept does not decide but names options with consequences; the adopter decides per secret. | Wahlpunkt | Eine Stelle, an der das Konzept nicht entscheidet, sondern Optionen mit Folgen nennt; der Übernehmende entscheidet je Geheimnis. |
| usage description | What a service publishes about how to use it, including the canonical names of its secrets. | Benutzungsbeschreibung | Was ein Dienst über seine Benutzung veröffentlicht, einschließlich der kanonischen Namen seiner Geheimnisse. |
| command word | An agreed value in the environment variable (such as `new`) that tells the service to regenerate a secret it owns; a command, not a secret. | Befehlswort | Ein vereinbarter Wert in der Umgebungsvariablen (etwa `new`), der den Dienst anweist, ein eigenes Geheimnis neu zu erzeugen; ein Befehl, kein Geheimnis. |
| builder | Whoever builds the service. | Erbauer | Wer den Dienst baut. |
| operator | Whoever runs the service. | Betreiber | Wer den Dienst betreibt. |
| consumer | Whoever calls the service, often an agent. | Konsument | Wer den Dienst aufruft, oft ein Agent. |
| load-bearing | A rule the rest of the concept depends on. | tragend | Eine Festlegung, von der der Rest des Konzepts abhängt. |
