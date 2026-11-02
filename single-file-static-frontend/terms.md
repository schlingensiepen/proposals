---
type: Proposal Terms
description: The terms used in this bundle, in English and German, each with a short explanation; the German column keeps the vocabulary the series was built with.
---

# Terms

| English term | Explanation | Deutscher Begriff | Erläuterung |
|---|---|---|---|
| Artifact | what gets deployed; here the contents of a directory of versioned files, with no intermediate step. | Artefakt | das, was ausgeliefert wird; hier ein Verzeichnisinhalt aus versionierten Dateien, ohne Zwischenschritt. |
| Build step | a procedure turning source files into a different, deployable result (bundling, transpiling, minifying). | Build-Schritt | ein Vorgang, der aus Quelldateien ein anderes, auszulieferndes Ergebnis erzeugt (Bündeln, Übersetzen, Verkleinern). |
| Data-free markup | delivered HTML containing no substantive content, only structure and labels; every request yields the same bytes. | Datenfreies Markup | ausgeliefertes HTML, das keine fachlichen Inhalte enthält, sondern nur Struktur und Beschriftungen; jeder Abruf liefert dieselben Bytes. |
| Hydration logic | the code in the artifact that fills the empty structure with data from the API at runtime; part of the form, not of the content: it describes where something goes, not what. | Hydrations-Logik | der Code im Artefakt, der die leere Struktur zur Laufzeit mit Daten aus der API füllt; Teil der Form, nicht des Inhalts: er beschreibt, wohin etwas kommt, nicht was. |
| Form | the empty structure with its labels; reads the same in every request (F2). | Form | die leere Struktur mit ihren Beschriftungen; steht in jedem Abruf gleich da (F2). |
| Content | everything that changes with the data; it arrives through the API, never in the document (F2). | Inhalt | alles, was sich mit den Daten ändert; kommt über die API, nie im Dokument (F2). |
| Credential | what a user presents to the API to prove who they are; the application is what obtains it (F2). | Nachweis | das, womit sich ein Benutzer gegenüber der API ausweist; die Anwendung ist das, was ihn beschafft (F2). |
| Trust boundary | a place where somebody must check who gets what; with this design the frontend is no longer one (F2). | Vertrauensgrenze | eine Stelle, an der geprüft werden muss, wer was bekommt; mit diesem Zuschnitt ist das Frontend keine mehr (F2). |
| Delivery path | the way from where the artifact is stored to the browser; no application code runs in it (F3). | Auslieferungspfad | der Weg vom Speicherort des Artefakts zum Browser; in ihm läuft kein Anwendungscode (F3). |
| Entry point | the one address at which the document is served. | Einstiegspunkt | die eine Adresse, unter der das Dokument liegt. |
| Fragment | the part of an address after the `#`; the browser does not ask the server about it; it is the client's business alone. | Fragment | der Teil einer Adresse hinter dem `#`; der Browser fragt ihn nicht beim Server nach; er ist ausschließlich Sache des Clients. |
| History API | the browser interface with which an application changes the displayed address without reloading; the addresses so produced look like paths and are requested from the server on the next call. | History-API | die Browser-Schnittstelle, mit der eine Anwendung die angezeigte Adresse ändert, ohne neu zu laden; die so entstandenen Adressen sehen aus wie Pfade und werden beim nächsten Aufruf beim Server angefragt. |
| Catch-all rule | a server configuration that serves the same document for arbitrary paths. | Catch-all-Regel | eine Serverkonfiguration, die auf beliebige Pfade dasselbe Dokument ausliefert. |
| Vendored third-party assets | third-party libraries, scripts and stylesheets that are files in the artifact instead of being loaded from an external server (F6). | Mitgelieferte Fremdmittel | Fremdbibliotheken, Skripte und Stile, die als Datei im Artefakt liegen, statt von einem fremden Server geladen zu werden (F6). |
| API envelope | the fixed wrapper of the API protocol, in which data travels only encoded (F7). | API-Umschlag | die feste Hülle des API-Protokolls, in der Daten nur kodiert reisen (F7). |
| Origin | scheme, host and port of an address; one origin for artifact and API avoids CORS (F8). | Origin | Schema, Host und Port einer Adresse; eine Origin für Artefakt und API vermeidet CORS (F8). |
| Recommendation | a rule with binding "should"; it concerns the interface, to which this design does not commit (F7, F8). | Empfehlung | eine Festlegung mit Verbindlichkeit „soll"; sie betrifft die Schnittstelle, auf die sich der Zuschnitt nicht festlegt (F7, F8). |
| rule | an entry with one answer; giving it up means no longer using the concept. | Festlegung | ein Eintrag mit einer Antwort; wer sie aufgibt, benutzt das Konzept nicht. |
