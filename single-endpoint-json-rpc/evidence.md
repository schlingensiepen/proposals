---
type: Proposal Evidence
description: A complete operation walked through from start to finish, and the honest state of play; the concept is fully specified but not yet proven in a running application.
---

# Evidence

## A complete operation, from start to finish

**A complete operation** — an entry with three images in an inventory system,
from start to finish. *Example; names and values are invented.*

First, file intake three times. It knows neither entry nor purpose:

```
POST /api/upload/
Content-Type: multipart/form-data; boundary=…
Cookie: session=…

  (field "file": the image file)
```

```json
{"handle": "9f2c4a1e-7b03-4d55-9c18-2e6a0b8f1d34"}
```

Only the application call establishes the connection — **one** call, three
handles, one indivisible outcome:

```
POST /api/
Content-Type: application/json
Cookie: session=…
```

```json
{"jsonrpc": "2.0",
 "method": "states.create",
 "params": {"item": 7,
            "text": "Christmas decorations, upper shelf",
            "photos": ["9f2c4a1e-…", "a71b0c93-…", "c04e58d7-…"]},
 "id": 1}
```

```json
{"jsonrpc": "2.0", "result": {"state": 42}, "id": 1}
```

If the third upload breaks off, **nothing** has come into being — there is no
half entry, only three orphaned files without a database row (F9).

An application error looks like this. The HTTP status is `200`; the three fields
carry three different pieces of information:

```json
{"jsonrpc": "2.0",
 "error": {"code": 409,
           "message": "Number \"12\" is already assigned to entry #7",
           "data": {"type": "number_taken"}},
 "id": 2}
```

A protocol error, by contrast, uses the reserved number space and carries only
the code and message the standard defines, no `data.type` (F5) — telling the
client that **its code** is wrong, not its data (F6):

```json
{"jsonrpc": "2.0",
 "error": {"code": -32601, "message": "Unknown method: items.serch"},
 "id": 3}
```

The images are fetched by the browser **not** through the API but directly from
the web server — a randomly named directory in which the file has kept its
original name (F8, F11):

```
GET /images/9f2c4a1e-7b03-4d55-9c18-2e6a0b8f1d34/shelf-top.jpg
```

**And the same route for a backup — with the three additions from F12.**
*Example; values invented.* A backup process commissions the compilation.
Because it takes minutes, no path comes back but a job handle:

```json
{"jsonrpc": "2.0", "method": "backup.commission", "params": {}, "id": 10}
```

```json
{"jsonrpc": "2.0", "result": {"job": "b23f9c01-…", "state": "running"}, "id": 10}
```

While compilation runs there is **no path** — only a state. Completion is what
creates it, in the same pattern as an attachment:

```json
{"jsonrpc": "2.0", "method": "backup.status", "params": {"job": "b23f9c01-…"}, "id": 11}
```

```json
{"jsonrpc": "2.0",
 "result": {"state": "done",
            "path": "/provision/7d4e2a95-…/inventory-2026-08-14.sqlite",
            "size": 41582592},
 "id": 11}
```

The process fetches the file directly from the web server — and **then reports
the successful retrieval**. The same call terminates the provision; file and
path disappear:

```json
{"jsonrpc": "2.0", "method": "backup.terminate", "params": {"job": "b23f9c01-…"}, "id": 12}
```

If the retrieval aborts, the file stays — and is cleared along the way at the
next `backup.commission`, provided its expiry has passed (F12, lazy
enforcement). That is the difference from the image above: its address may
remain, because it exposes an image. This one exposes the entire dataset.

## Origin

**Reference implementation: omitted, because none exists yet (status: draft).**
The concept is fully specified and described in contract tests, but the running
application it comes from runs an older version and does not yet implement it.
The statements in this concept are therefore **thought through, not proven** —
in particular, the claim that F9 actually establishes atomicity is so far an
argument only.
