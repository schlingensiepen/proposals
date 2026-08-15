# Anfrage: ein kleines Werkzeug, nennen wir es „imap-sync"

*(Beschreibung aus Anwendersicht, so wie der Auftraggeber sie mündlich gibt.)*

**Worum es geht.** Ich habe alte Postfächer bei verschiedenen Anbietern, aus denen ich
Mails in mein Hauptpostfach übernehmen möchte — nicht einmalig, sondern immer wieder,
solange die alten Adressen noch Post bekommen. Konkret: Was in einem bestimmten Ordner
des alten Postfachs liegt, soll auch in einem bestimmten Ordner meines Hauptpostfachs
liegen. Was schon drüben ist, darf nicht noch einmal kopiert werden — ich will keine
Dubletten. Und gelöscht wird nie etwas, weder hier noch dort.

**Wie ich es benutzen will.** Ich sitze nicht davor. Ein Skript oder ein Agent von mir
stößt den Abgleich an, wenn es gerade passt — nachts, nach dem Urlaub, wann auch immer.
Danach will derselbe Agent wissen können, ob der Abgleich durch ist und was dabei
herauskam: wie viele Mails übernommen wurden, ob etwas schiefging. Wenn ein Anbieter
gerade zickt oder die Verbindung abbricht, soll das Werkzeug beim nächsten Anlauf
einfach weitermachen und nicht wieder bei null anfangen.

**Wer es betreibt.** Ich, auf meinem kleinen Server. Ich hätte gern, dass ich für jede
Kombination — *dieses* Quellpostfach, *dieser* Ordner, *jenes* Zielpostfach, *jener*
Ordner — einmal etwas einrichte und meinem Skript dann sage, wie es drankommt. Von den
Zugangsdaten meiner Postfächer soll mein Skript möglichst nichts wissen müssen.

**Was mir wichtig ist.** Es soll klein bleiben. Ich will kein Portal, keine
Benutzerverwaltung und nichts, was ich alle zwei Wochen aktualisieren muss, damit es
nicht unsicher wird. Wenn ich in einem halben Jahr eine weitere alte Adresse dazunehme,
will ich das in fünf Minuten eingerichtet haben.
