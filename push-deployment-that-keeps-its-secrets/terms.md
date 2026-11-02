---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| Component | a part of the system that is delivered independently: application, background process, backup service. | Komponente | ein Teil des Systems, der eigenständig ausgeliefert wird: Anwendung, Hintergrundprozess, Sicherungsdienst. |
| Instance | a concrete target of a component: this web space, this machine. A component can have many instances. | Instanz | ein konkretes Ziel einer Komponente: dieser Webspace, dieser Rechner. Eine Komponente kann viele Instanzen haben. |
| Instance directory | the folder ignored by version control that holds everything that makes up this one instance: address, credentials, settings, created secrets. | Instanzverzeichnis | der gitignorierte Ordner mit allem, was diese eine Instanz ausmacht: Adresse, Zugangsdaten, Einstellungen, erzeugte Geheimnisse. |
| Instance configuration | the settings of one instance inside its instance directory; the system secrets are backed up beside it. | Instanzkonfiguration | die Einstellungen einer Instanz in ihrem Instanzverzeichnis; neben ihr werden die Systemgeheimnisse gesichert. |
| Host system | the external machine that is delivered to. | Trägersystem | der fremde Rechner, auf den ausgeliefert wird. |
| Access secret | what the script uses to get onto the host system. | Zugangsgeheimnis | womit das Skript auf das Trägersystem kommt. |
| System secret | what comes into being during delivery and takes effect in the system. | Systemgeheimnis | was beim Ausliefern entsteht und im System wirkt. |
| Window | a time-limited state in which operations are allowed that are forbidden in running operation. | Fenster | ein befristeter Zustand, in dem Vorgänge erlaubt sind, die im laufenden Betrieb verboten sind. |
| Production configuration | the configuration of the running system on the host system; created by the script when missing, never overwritten without `--force`. | Produktive Konfiguration | die Konfiguration des laufenden Systems auf dem Trägersystem; vom Skript angelegt, wenn sie fehlt, nie ohne `--force` überschrieben. |
| Runtime data, store | what arises on the host system at runtime: the store, attachments, backups; locked in the core of the script. | Laufzeitdaten, Bestand | was auf dem Trägersystem zur Laufzeit entsteht: Bestand, Anhänge, Sicherungen; im Kern des Skripts gesperrt. |
| Entry point | the tool above the individual scripts that runs, creates and lists targets. | Einstieg | das Werkzeug über den Einzelskripten, das Ziele ausführt, anlegt und auflistet. |
| Sample configuration | the versioned template beside each delivery script, copied by the entry point when a target is created. | Musterkonfiguration | die versionierte Vorlage neben jedem Auslieferungsskript, die der Einstieg beim Anlegen eines Ziels kopiert. |
| Manual operation | doing a delivery by hand, following the guide, when the script cannot be used. | Handbetrieb | eine Auslieferung von Hand, nach der Anleitung, wenn das Skript nicht benutzt werden kann. |
| Builder | anyone who writes the delivery scripts and their tooling. | Erbauer | wer die Auslieferungsskripte und ihr Werkzeug schreibt. |
| Operator | the deliverer: anyone who owns and keeps the instance directories and delivers from their own machine. | Betreiber | der Ausliefernde: wer die Instanzverzeichnisse besitzt und verwahrt und vom eigenen Rechner ausliefert. |
| rule | an entry with one answer; giving it up means no longer using the concept. | Festlegung | ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
| choice | an entry where the concept does not decide but names options with consequences; the adopter decides and records. | Wahlpunkt | eine Stelle, an der das Konzept nicht entscheidet, sondern Optionen mit Folgen nennt; der Übernehmende entscheidet und hält fest. |
