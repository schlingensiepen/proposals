---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| operating parameter | A value the service needs in order to run, set before the start: port, directory, target address, log level. Not state, not a secret. | Betriebsparameter | Ein Wert, den der Dienst zum Laufen braucht und der vor dem Start gesetzt wird. Kein Zustand, kein Geheimnis. |
| configuration | The whole of the effective operating parameters at start, however many places they came from. | Konfiguration | Die Gesamtheit der wirksamen Betriebsparameter beim Start, gleich aus wie vielen Orten sie zusammenkommt. |
| place | A location where a value can be set: command line, environment, user-level file, system-level file. | Ort | Eine Stelle, an der ein Wert stehen kann: Aufruf, Umgebung, benutzernahe Datei, systemweite Datei. |
| precedence | The fixed order in which places are consulted; the first one that is set wins. | Präzedenz | Die feste Reihenfolge, in der Orte befragt werden; der erste belegte gewinnt. |
| rank | A place's position in the precedence. | Rang | Die Position eines Orts in der Präzedenz. |
| judgment | The decision whether a configuration is fit to start on; made by the check, confirmed by the start. | Urteil | Die Entscheidung, ob eine Konfiguration zum Start taugt; gefällt von der Prüfung, bestätigt vom Start. |
| check command | The invocation that makes the judgment and shows the effective values with their sources without starting the service. | Prüfkommando | Der Aufruf, der das Urteil fällt und die wirksamen Werte mit Quelle zeigt, ohne den Dienst zu starten. |
| default | The value in effect when no place is set. | Vorgabe | Der Wert, der wirkt, wenn kein Ort belegt ist. |
| empty value | A place is set, but to nothing; distinct from "no place is set". | leerer Wert | Ein Ort ist belegt, aber mit nichts; zu unterscheiden von „kein Ort belegt". |
| rule | An entry with one answer; giving it up means no longer using the concept. | Festlegung | Ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
| choice | An entry where the concept does not decide but names options with consequences; the adopter decides per setting. | Wahlpunkt | Eine Stelle, an der das Konzept nicht entscheidet, sondern Optionen mit Folgen nennt; der Übernehmende entscheidet je Einstellung. |
| builder | Anyone who builds the service. | Erbauer | Wer den Dienst baut. |
| operator | Anyone who runs the service. | Betreiber | Wer den Dienst betreibt. |
| adopter | Anyone who finds the service later and takes it over. | Übernehmender | Wer den Dienst später vorfindet und übernimmt. |
| load-bearing | A rule the rest of the concept depends on. | tragend | Eine Festlegung, von der der Rest des Konzepts abhängt. |
| independent | A rule that holds on its own and is useful outside this concept. | unabhängig | Eine Festlegung, die für sich gilt und auch außerhalb dieses Konzepts nützt. |
