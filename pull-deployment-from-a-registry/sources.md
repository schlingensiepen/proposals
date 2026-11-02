---
type: Proposal Sources
description: What this proposal builds on; the three concepts it imports, the alternative and a related proposal of the series, the reference service and two external sources.
sources:
  - id: oci-distribution-spec
    url: https://github.com/opencontainers/distribution-spec/blob/main/spec.md
    title: Open Container Initiative, Distribution Specification
  - id: rfc7838
    url: https://www.rfc-editor.org/rfc/rfc7838.html
    title: IETF RFC 7838, HTTP Alternative Services
---

# Sources

## Imports

* `build-stamp` (at F2 and F7): used for the source revision in the image and its report by the running service. The build stamp is the precondition of F7 and of the last assertion of F2; it is not repeated here but used. This concept can be applied without the import as long as there is an image that reports its build state.
* `where-a-secret-comes-from` (at F2, and for the first secret of a pulled service): where a start secret comes from and in which ranking.
* `proven-from-outside` (at F2, and at the acceptance of the handover to the operator): used for the red run of every smoke-test assertion and for the standard by which the handover to an unknown operator counts as proven (see the evidence); it is not repeated here.

## Related

* `push-deployment-that-keeps-its-secrets` (alternative-to): the alternative; choose one per case. The line does not run between pushing and pulling but between having a way into the target and having none; a system of several components can need both side by side. This concept is complete without it.
* `service-as-single-artifact` (recommended-with): F6 builds on its principle for deployment files. Deepening, not a prerequisite.
* `self-describing-services` (recommended-with): named by that proposal from its side; see there.
* `admin-surface-over-http` (recommended-with): named by that proposal from its side; see there.

All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

External sources accessed on 2026-09-22.

* The author's mail-filing service: the evidence — build pipeline, container description and delivery directory, whose state of 2026-09-22 was inspected. Its repository is not publicly accessible. The incidents appear in the problem, the rules and the evidence in stripped-down form.
* Open Container Initiative, Distribution Specification [^oci-distribution-spec]: the terms of this bundle. The manifest is fetched under `/v2/<name>/manifests/<tag-or-digest>`, where the reference is either the digest of the manifest or a tag (section *Pulling manifests*); a tag must match the expression `[a-zA-Z0-9_][a-zA-Z0-9._-]{0,127}` — the slash is not in it, and that is the reason for F3 (same place); a tag is "a custom, human-readable pointer to a manifest", and a digest can carry zero, one or many tags (section *Definitions*) — the split into moving and immutable names in F3 is that of this specification.
* IETF RFC 7838, HTTP Alternative Services [^rfc7838]: the mechanism behind F8. An origin server can advertise alternative services to clients through the `Alt-Svc` header (section 3); without a parameter an advertisement counts as fresh for 24 hours, with the parameter `ma` longer (section 3.1); and a client whose connection to the alternative service fails or does not answer **may** ("MAY") fall back to the origin (section 2.4) — it does not have to, and the incident rests on that.

[^oci-distribution-spec]: https://github.com/opencontainers/distribution-spec/blob/main/spec.md
[^rfc7838]: https://www.rfc-editor.org/rfc/rfc7838.html
