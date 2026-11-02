---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| Queue | the place between intake and execution where jobs wait; here a directory tree. | Warteschlange | der Ort zwischen Annahme und Ausführung, an dem Aufträge warten; hier ein Verzeichnisbaum. |
| Job | the smallest unit of work the queue carries; here exactly one file. | Auftrag | die kleinste Arbeitseinheit, die die Warteschlange transportiert; hier genau eine Datei. |
| Intake | the part of the service that accepts a request, validates and translates it once, and enqueues the job. | Annahmestelle | der Teil des Dienstes, der eine Anfrage entgegennimmt, einmal prüft und übersetzt und den Auftrag einreiht. |
| Worker | a process or thread that claims jobs from the queue and carries them out. | Arbeiter | ein Prozess oder Faden, der Aufträge aus der Warteschlange übernimmt und ausführt. |
| State folder | a directory whose name denotes the state of the jobs in it. | Zustandsordner | ein Verzeichnis, dessen Name den Zustand der darin liegenden Aufträge bezeichnet. |
| Staging folder | the directory in which a job file comes into being before it is complete; its content is never part of the flow. In the mail server tradition it is called `tmp`. | Übergabeordner | das Verzeichnis, in dem eine Auftragsdatei entsteht, bevor sie vollständig ist; ihr Inhalt ist nie Teil des Ablaufs. In der Mailserver-Tradition heißt er `tmp`. |
| Staging · waiting · in progress · done · failed | the five state folders that have proven themselves. | Übergabe · Wartend · In Arbeit · Erledigt · Gescheitert | die fünf Zustandsordner, die sich bewährt haben. |
| Atomic rename | the operating system's guarantee that on `rename()` within one file system a file appears at the target either completely or not at all; there is no intermediate state and no half-visible file. | Atomares Umbenennen | die Betriebssystem-Zusage, dass eine Datei bei `rename()` innerhalb eines Dateisystems entweder vollständig am Zielort erscheint oder gar nicht; es gibt keinen Zwischenzustand und keine halb sichtbare Datei. |
| Claim | the takeover of a job by a worker, carried out here by the rename into the in-progress folder; the success of the rename *is* the claim. | Anspruch (Claim) | die Übernahme eines Auftrags durch einen Arbeiter, hier vollzogen durch das Umbenennen in den Bearbeitungsordner; das Gelingen des Umbenennens *ist* der Anspruch. |
| Restart rule | the cleanup operation at start that heals a crash in the middle of processing. | Wiederanlauf-Regel | die Aufräumoperation beim Start, die einen Absturz mitten in der Verarbeitung heilt. |
| Journal | the state folders for what is done and what has failed, which become the history; whether they are cleared by age or under a quota is the choice W1. | Journal | die Zustandsordner für Erledigtes und Gescheitertes, die damit zur Historie werden; ob sie nach Alter oder Quote geräumt werden, ist Wahlpunkt W1. |
| Payload data | the content the jobs carry, including uploaded attachments; uploads are never deleted automatically, the journal only as decided under W1. | Nutzdaten | der Inhalt, den die Aufträge tragen, einschließlich hochgeladener Anhänge; Uploads werden nie automatisch gelöscht, das Journal nur nach Wahlpunkt W1. |
| Pool | a separate, durable store for attachments that jobs only refer to. | Pool (Dateipool) | ein getrennter, dauerhafter Speicher für Anhänge, auf den Aufträge nur verweisen. |
| Idempotency marker | an empty or minimal file whose exclusive creation (`O_EXCL`) answers the question "have I accepted this job before?" without any further storage. | Idempotenz-Marker | eine leere oder minimale Datei, deren exklusives Anlegen (`O_EXCL`) die Frage „habe ich diesen Auftrag schon einmal angenommen?" ohne weiteren Speicher beantwortet. |
| Health endpoint | the unauthenticated endpoint that says only whether the service is running. | Gesundheitspunkt | der nicht authentifizierte Endpunkt, der nur sagt, ob der Dienst läuft. |
| At-least-once | the delivery guarantee of the pattern: after a crash a job may run again, it is never lost. | Mindestens-einmal | die Zusage des Musters: Nach einem Absturz kann ein Auftrag erneut laufen, er geht nie verloren. |
| Builder | whoever builds the queue into a service. | Erbauer | wer die Warteschlange in einen Dienst einbaut. |
| Operator | whoever runs the service and intervenes when something goes wrong. | Betreiber | wer den Dienst betreibt und im Störfall eingreift. |
| Caller | whoever submits a job and wants to know what became of it. | Aufrufer | wer einen Auftrag einreicht und wissen will, was aus ihm geworden ist. |
| rule | An entry with one answer; giving it up means no longer using the concept. | Festlegung | Ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
| choice | An entry with more than one legitimate answer; decided per service by whoever adopts the concept, and recorded. | Wahlpunkt | Ein Eintrag mit mehr als einer legitimen Antwort; je Dienst entschieden von dem, der das Konzept übernimmt, und festgehalten. |
