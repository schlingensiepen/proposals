---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| Function | the one thing the service can do, describable in one sentence; its purpose. | Funktion | das eine, was der Dienst kann, in einem Satz beschreibbar; sein Zweck. |
| Main method | the call that triggers the function. | Hauptmethode | der Aufruf, der die Funktion auslöst. |
| Helper method | a call without which the function does not work or is not verifiable: preparation (file upload, say), status enquiry, cleaning up. Helper methods are not functions of their own and share the access of the main method. | Hilfsmethode | ein Aufruf, ohne den die Funktion nicht geht oder nicht überprüfbar ist: Vorbereitung (etwa Datei-Upload), Statusabfrage, Aufräumen. Hilfsmethoden sind keine eigenen Funktionen und teilen den Zugang der Hauptmethode. |
| Artifact | the one built, versioned, shippable unit that is replaced as a whole: a binary, a package or a directory (F2). | Artefakt | die eine gebaute, versionierte, ausgelieferte Einheit, die als Ganzes ausgetauscht wird: Binärdatei, Paket oder Verzeichnis (F2). |
| Mode | an operating mode of the same artifact, chosen at invocation. | Modus | eine Betriebsart desselben Artefakts, gewählt beim Aufruf. |
| Service mode | the long-running mode that serves the network surface. | Dienstmodus | der lange laufende Modus, der die Netzfläche bedient. |
| Command | an invocation of the artifact that ends when its task is done, for configuration, checking, state display or cleaning up; through a subcommand or an entry point of its own within the same artifact, never through a separately built tool. The artifact is the thing, the command its use. | Kommando | ein Aufruf des Artefakts, der nach der Aufgabe endet, für Konfiguration, Prüfung, Zustandsanzeige oder Aufräumen; über einen Unterbefehl oder einen eigenen Einstiegspunkt desselben Artefakts, nie über ein getrennt gebautes Werkzeug. Das Artefakt ist das Ding, das Kommando sein Gebrauch. |
| Access | the artifact with which a caller uses the function (as a rule a token); it embodies a concrete, preconfigured variant of the function. | Zugang | das Artefakt, mit dem ein Aufrufer die Funktion benutzt (in der Regel ein Token); es verkörpert eine konkrete, vorkonfigurierte Ausprägung der Funktion. |
| Management surface | any possibility, reachable over the network, of configuring the service, inspecting it or changing accesses; not provided for in this concept. | Verwaltungsfläche | jede über das Netz erreichbare Möglichkeit, den Dienst zu konfigurieren, zu inspizieren oder Zugänge zu ändern; in diesem Konzept nicht vorgesehen. |
| State | everything that arises or changes in operation: store, operating data, queues; is outside the artifact. Distinct from the configuration, which is set before the start and read at the start. | Zustand | alles, was im Betrieb entsteht oder sich im Betrieb ändert: Bestand, Betriebsdaten, Warteschlangen; liegt außerhalb des Artefakts. Abgegrenzt von der Konfiguration, die vor dem Start gesetzt und beim Start gelesen wird. |
| Store | The variants and their accesses that the setup command creates; state, not configuration (F12). | Bestand | Die Ausprägungen und ihre Zugänge, die das Einrichtungskommando anlegt; Zustand, nicht Konfiguration (F12). |
| Permission cut | the separation of write permissions between the modes: configuration is written only by commands, the service mode only reads it; enforced through users and file permissions (F8). | Rechte-Schnitt | die Trennung der Schreibrechte zwischen den Modi: Konfiguration schreiben nur die Kommandos, der Dienstmodus liest sie nur; durchgesetzt über Benutzer und Dateirechte (F8). |
| Process form | how the artifact is started and supervised: systemd unit, container runtime, something else; interchangeable for this concept. | Prozessform | wie das Artefakt gestartet und beaufsichtigt wird: systemd-Unit, Container-Laufzeit, etwas anderes; für dieses Konzept austauschbar. |
| Reverse proxy | the instance placed in front that serves public names and TLS and passes on to the local endpoint of the service. | Reverse Proxy | die vorgeschaltete Instanz, die öffentliche Namen und TLS bedient und an den lokalen Endpunkt des Dienstes weiterreicht. |
| Check command | the command that assesses the configuration without starting the service. | Prüfkommando | das Kommando, das die Konfiguration bewertet, ohne den Dienst zu starten. |
| Version drift | the state in which two separately built delivery artifacts of the same system carry different versions of the same knowledge. | Versionsdrift | der Zustand, in dem zwei getrennt gebaute Erzeugnisse desselben Systems verschiedene Fassungen desselben Wissens tragen. |
| rule | An entry with one answer; giving it up means no longer using the concept. | Festlegung | Ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
| choice | An entry where the concept does not decide but names options with consequences; the adopter decides and records. | Wahlpunkt | Eine Stelle, an der das Konzept nicht entscheidet, sondern Optionen mit Folgen nennt; der Übernehmende entscheidet und hält fest. |
