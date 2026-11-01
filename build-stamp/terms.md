---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| Build stamp | the source revision the artifact was built from, written into the artifact and reported by the version command. | Baustempel | die Quellrevision, aus der das Artefakt gebaut wurde, in das Artefakt eingetragen und vom Versionskommando genannt. |
| Source revision | the revision of the source a build was made from. | Quellstand | die Revision, aus der gebaut wurde. |
| Artifact | the buildable unit that is replaced as a whole: a binary, a package or a directory. | Artefakt | die baubare und als Ganzes austauschbare Einheit: Binärdatei, Paket oder Verzeichnis. |
| Deliverable | the published delivery artifact, for example an image in a registry. | Erzeugnis | das veröffentlichte Auslieferungsartefakt, etwa ein Abbild in einer Registry. |
| Version command | the invocation of the artifact that reports its release and its build. | Versionskommando | der Aufruf des Artefakts, der Fassung und Baustand nennt. |
| Release number | the version number a person assigns; it does not move between two builds. | Fassungsnummer | die vom Menschen vergebene Versionsnummer; bewegt sich zwischen zwei Bauten nicht. |
| Build pipeline | the chain that builds the deliverable from the source and publishes it. | Bauleitung | die Kette, die das Erzeugnis aus der Quelle baut und veröffentlicht. |
| Smoke test | starting the built deliverable in the build pipeline before publishing, requiring that it reports the revision built. | Rauchtest | der Start des gebauten Erzeugnisses in der Bauleitung vor der Veröffentlichung, der verlangt, dass es den gebauten Stand nennt. |
| Running instance | the process or container that is running right now, as distinct from the installed or published artifact. | Laufende Instanz | der Prozess oder Behälter, der gerade läuft, im Unterschied zum installierten oder veröffentlichten Artefakt. |
| One-off invocation | a short invocation of the same artifact against the running instance, for example inside the running container. | Einmal-Aufruf | ein kurzer Aufruf desselben Artefakts gegen die laufende Instanz, etwa im laufenden Behälter. |
| Readiness point | the unauthenticated point that says nothing but "running". | Bereitschaftspunkt | der unauthentifizierte Punkt, der nur „läuft" sagt. |
| Companion file | a file beside the artifact that does not travel with it. | Begleitdatei | eine Datei neben dem Artefakt, die nicht mit ihm reist. |
| Management surface | any possibility, reachable over the network, of configuring a service, inspecting it or changing accesses. | Verwaltungsfläche | jede über das Netz erreichbare Möglichkeit, einen Dienst zu konfigurieren, zu inspizieren oder Zugänge zu ändern. |
| rule | An entry with one answer; giving it up means no longer using the concept. | Festlegung | Ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
