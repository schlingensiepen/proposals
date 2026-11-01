---
type: Proposal Sources
description: What this proposal builds on; the two concepts it imports, related proposals of the series, and external sources.
sources:
  - id: openapi
    url: https://spec.openapis.org/oas/latest.html
    title: OpenAPI Specification, version 3.2.0 (19 September 2025)
  - id: mcp
    url: https://modelcontextprotocol.io/
    title: Model Context Protocol
  - id: fielding-rest
    url: https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
    title: "Roy Thomas Fielding: Architectural Styles and the Design of Network-based Software Architectures, chapter 5 (REST), 2000"
  - id: llms-txt
    url: https://llmstxt.org/
    title: "Jeremy Howard: The /llms.txt file (version v2)"
  - id: rfc8615
    url: https://www.rfc-editor.org/rfc/rfc8615
    title: "RFC 8615: Well-Known Uniform Resource Identifiers (URIs)"
  - id: graphql-introspection
    url: https://graphql.org/learn/introspection/
    title: "GraphQL: Introspection"
  - id: grpc-reflection
    url: https://grpc.io/docs/guides/reflection/
    title: "gRPC: Reflection"
  - id: openrpc
    url: https://open-rpc.org/
    title: OpenRPC
  - id: wikijs-api
    url: https://docs.requarks.io/dev/api
    title: "Wiki.js: GraphQL API (developer documentation)"
---

# Sources

## Imports

* `where-a-secret-comes-from` (imports, at F13): the ranking of sources for a secret, environment, pass-through file, once-only channel, and what a ranking obliges. The usage description prescribes it to the consumer; for the consumer the once-only channel is the context window.
* `proven-from-outside` (imports, at the checks in the build and the reading by somebody without inside knowledge): the proofs this concept relies on, that a check demanded here tests anything at all and that a description works for a reader who has only the URL. This concept demands the checks and the outside reading; how they are proven follows that concept.

## Related

* `capability-tokens-for-agents` (recommended-with, at F12): the deeper justification of the narrow token cut. Not needed for understanding; F12 summarizes what is used here.
* `service-as-single-artifact` (recommended-with): how a small service is built; this concept says how it describes itself. Separate matters; either can be applied without the other.
* `immutable-decision-records` and `decision-records-with-madr` (recommended-with, at F17): the place hindsight goes to, decision records that travel with the source code and are replaced, never changed.
* `pull-deployment-from-a-registry` (recommended-with, at F21): a service that comes up by being pulled from a registry and started is in the empty state at every instance, as the normal case; F21 says that what it answers then is decided.
* `get-only-exit` (recommended-with): named by that proposal from its side; see there.
* `self-contained-repos` (overlaps-with): named by that proposal from its side; see there.
* `admin-surface-over-http` (recommended-with): named by that proposal from its side; see there.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* OpenAPI Specification [^openapi]: the "generated contract" from the concept, which F6 declares a product and enriches; its description and example fields are the places the gate from F6 demands. It also supports the flip condition according to which the durability of the interface description depends on its falling out of the code.
* Model Context Protocol [^mcp]: the model for the parallel in the concept; a server describes its methods at runtime, the client queries them (discovery instead of distribution). This concept carries the same principle over to the web API, which needs no connecting.
* Fielding, REST [^fielding-rest]: the origin of the hypermedia idea F7 invokes, responses that name the next steps themselves. F7 redeems this old promise for a consumer who actually does read at runtime.
* The /llms.txt file [^llms-txt]: the closest existing convention for agent-readable self-disclosure and thus the benchmark for the entry page from F2. It shows Markdown with links as the carrier form; this concept goes beyond it in that the disclosure is rendered by the service at runtime (F5) and loaded in stages (F2).
* RFC 8615 [^rfc8615]: reserves the path prefix `/.well-known/` and is thus the pattern on which a possible path convention for the cascade would be modeled (not yet settled, see the scope).
* GraphQL introspection [^graphql-introspection]: native self-description of the mechanics in another dialect, including description fields on the schema; the point of attachment for the analogy in F6. It is also the interface mechanics of the wiki platform in the evidence.
* gRPC reflection [^grpc-reflection]: native self-description of the mechanics with gRPC; completeness of the mechanics without curation, precisely the gap F6 fills.
* OpenRPC [^openrpc]: the contract mechanism for JSON-RPC 2.0 that F6 names as a dialect to be enriched analogously; its `rpc.discover` is the counterpart there to the runtime query.
* Wiki.js GraphQL API [^wikijs-api]: the platform the precursor in the evidence refers to, and the type of existing API for which F15 provides the façade.
* The author's wiki toolkit, repository `syncwiki` (not publicly accessible): Evidence 2, the precursor in operation, 140,000 nodes, the count contradiction "12" next to "13".
* The author's mail filing service, repository `imap-service` (not publicly accessible): Evidence 1, the reference implementation, including the case behind F21.
* The author's dissertation, 2007: self-description services with quality attributes in a peer-to-peer system with grid functions; Evidence 3 and the origin of the quality dimension the scope expressly does not cover. Bibliographic details are not included in this version.

**Placement.** The conceptual line of this concept runs between two traditions. The one is hypermedia: responses that name the next steps themselves. It came too early, because human developers did not want to discover anything at runtime; agents do it without complaint, and F7 redeems the promise late. The other is the generated self-description of the mechanics in individual dialects (OpenAPI, GraphQL, gRPC, OpenRPC): it delivers completeness, but no weighting; the consumer learns what exists, not what belongs together and what has which consequences. This concept adds in three places: curation *inside* the carrier instead of next to it (F6), a behavioral level alongside the mechanics (F11), and a staged entry that puts the decision before the loading (F2). The Model Context Protocol shows the same principle for tools, llms.txt a leaner form of the same idea without runtime rendering.

[^openapi]: https://spec.openapis.org/oas/latest.html
[^mcp]: https://modelcontextprotocol.io/
[^fielding-rest]: https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
[^llms-txt]: https://llmstxt.org/
[^rfc8615]: https://www.rfc-editor.org/rfc/rfc8615
[^graphql-introspection]: https://graphql.org/learn/introspection/
[^grpc-reflection]: https://grpc.io/docs/guides/reflection/
[^openrpc]: https://open-rpc.org/
[^wikijs-api]: https://docs.requarks.io/dev/api
