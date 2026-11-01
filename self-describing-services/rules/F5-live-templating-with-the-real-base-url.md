---
type: Proposal Rule
description: All examples are rendered with the host under which the service is currently being addressed.
number: F5
binding: must
role: independent
applies_to: [builder]
---

# F5 · Live templating with the real base URL

**Applies to:** builder.

**What holds.** All examples in entry page, usage description, and interface description are rendered with the host under which the service is currently being addressed.

**Why.** Every example is thereby copy-paste ready for exactly this installation. Placeholders are a source of error precisely with the consumer who takes things literally.

**Obvious and rejected.** *`example.com` placeholders with a "replace with your host" footnote* — agents actually do call placeholders. *Rendering a configured "official" URL* — behind proxies and with tenants it does not match the request.

**How you notice it is violated.** The logs contain requests against `example.com`.
