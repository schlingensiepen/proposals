---
type: Proposal Rule
description: Every rejection names the correct path, concretely, with the next step.
number: F7
binding: must
role: independent
applies_to: [builder]
---

# F7 · Teaching error messages

**Applies to:** builder.

**What holds.** Every rejection names the correct path — concretely, with the next step. At the traffic service, the upload endpoint answers an embedded image not with "invalid request" but with:

```json
{
  "error": "inline_image_not_accepted",
  "message": "Images cannot be embedded in a report. Upload the image first:
              POST https://traffic.example.org/api/v1/files (returns a file_id),
              then reference it as \"photo\": {\"file_id\": \"...\"}."
}
```

**Why.** The moment of failure is the moment of highest attention: with the agent, the correction instruction is in the context window exactly when it is needed — self-description at the point of failure. Error texts are interface description and deserve the same editorial care. It is at the same time the redemption of an old idea: HATEOAS wanted responses that name the next steps themselves — it only came too early, because human developers did not want to discover anything at runtime; agents do it without complaint.

**Considered and rejected.** *Generic error codes with a pointer to the documentation* — a break in the medium; the agent guesses instead of reading. *Error texts as an implementation detail* — then they explain the internal state instead of the way out.

**How you notice it is violated.** An agent repeats the same failing call several times in a row — the response has taught him nothing.
