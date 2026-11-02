---
type: Proposal Example
description: The way of a reviewing agent through a GET-only surface, depositing a message with an attachment in a mailbox, retold in today's path form; all values fictional, for illustration only.
---

# A message with attachment, GET only

What follows is the way of the reviewing agent from the evidence,
retold in the path form, which did not exist at the time: the steps are
its steps, the addresses today's, all values fictional. It is here for
illustration and proves nothing the evidence does not already prove.

The agent gets one sentence and two things from its user: *"Put the
review note with the report as an attachment into my mailbox; you can
only fetch addresses"*, the address `https://mail.example.org/get/v1`,
and a token, which it stores in its environment and inserts into every
address without ever quoting it. It fetches the description and reads
first what this costs it: the token will be in every address, so in
logs and in its own history; it tells its user so in one sentence and
carries on, because its tool leaves it no choice
([F11](../rules/F11-the-price-named.md)). The first call with the token
is `…/get/v1/<token>/me`; the answer comes as text (`folder:
INBOX/Review notes`, plus the limits) and not as structure, because its
tool said nothing about `Accept`
([F4](../rules/F4-reading-mirrors-the-main-surface.md)).

The attachment is 9 kilobytes. The agent opens an upload,
`…/<token>/newfile/report.pdf`, and gets an upload identifier whose
source the description had named in the same breath ("answers
`upload_id`", [F12](../rules/F12-the-description-is-the-only-document.md)).
It splits the file into three parts, encodes each as base64url and sends
them as `…/upload/<id>/part/1/<…>`, `…/part/2/<…>`, `…/part/3/<…>`; the
second part twice, because its tool swallowed the first answer; the
repetition has no consequence ([F5](../rules/F5-the-upload-in-parts.md),
[F10](../rules/F10-repeatability-without-headers.md)). Then
`…/upload/<id>/finish`: only now does the file arise in the store; the
answer names identifier, size and checksum, and the checksum matches
that of its local file.

It creates a composition, `…/<token>/compose`, and fills it field by
field: `…/compose/<id>/from/agent@example.org`,
`…/to/user@example.org`, `…/subject/<base64url of "Review note
Q3/2026">` (the subject contains a slash, and because it travels
encoded, it arrives intact, [F3](../rules/F3-two-encoding-rules.md)),
`…/text/<base64url of the text>`, `…/attach/<file-id>`. A second
recipient would be a second call, not a second parameter
([F9](../rules/F9-no-guessing.md)). Then it shows the composition,
`…/compose/<id>`, and puts it before its user: recipients, subject,
attachment with name. Only after their consent does it call the one
address that takes effect: `…/compose/<id>/deposit/review-note-01`
([F6](../rules/F6-the-composition.md)). The answer is a receipt with a
message identifier; it says "accepted", not "delivered". The connection
drops before it reads it; it calls the same address again and gets the
same receipt; the mailbox holds one message, not two (F10). With
`…/<token>/message/<message-id>` it follows the message until its state
reads `done`.

What does not appear in this run is the part the costs present:
seventeen addresses with the token in them (and every further status
query one more) are now in the proxy's access log, in the tool's
history and in the transcript of the session. The user was told
beforehand, the token is limited to one folder, and the operator knows
it should live briefly. This concept offers no more than that, and it
does not claim more.
