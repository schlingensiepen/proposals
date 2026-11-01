---
type: Proposal Sources
description: What this proposal builds on; the concept it imports, a related proposal of the series, and external sources on capabilities, the confused deputy, attenuation and prompt injection.
sources:
  - id: dennis-van-horn-1966
    url: https://doi.org/10.1145/365230.365252
    title: Jack B. Dennis, Earl C. Van Horn, Programming Semantics for Multiprogrammed Computations, Communications of the ACM 9(3), 1966
  - id: hardy-confused-deputy
    url: https://css.csail.mit.edu/6.858/2015/readings/confused-deputy.html
    title: Norm Hardy, The Confused Deputy (or why capabilities might have been invented), ACM SIGOPS Operating Systems Review, 1988
  - id: macaroons
    url: https://research.google/pubs/macaroons-cookies-with-contextual-caveats-for-decentralized-authorization-in-the-cloud/
    title: Arnar Birgisson et al., Macaroons, Cookies with Contextual Caveats for Decentralized Authorization in the Cloud, NDSS 2014
  - id: owasp-llm01
    url: https://genai.owasp.org/llmrisk/llm01-prompt-injection/
    title: OWASP Gen AI Security Project, LLM01:2025 Prompt Injection
---

# Sources

Locations checked on 2026-08-14 unless noted otherwise.

## Imports

* `where-a-secret-comes-from` (at F6): the ranking by which a secret is obtained and kept, environment, then a pass-through file, last the once-only channel; for the agent, the once-only channel is the context window.

## Related

* `self-describing-services` (generalizes): the worked application of this concept to self-describing web services, shown in the evidence. Further depth, not a prerequisite for understanding.
* `invitation-instead-of-login` (overlaps-with): named by that proposal from its side; see there.
* `admin-surface-over-http` (recommended-with): named by that proposal from its side; see there.

The two works of the author named in the evidence, the wiki toolkit and the mail filing service, live in repositories that are not publicly accessible. All proposals live in the same repository; see its root README for the register and the links. Which proposals build on this one is listed in [`used-by.md`](./used-by.md).

## External sources

* Dennis and Van Horn, *Programming Semantics for Multiprogrammed Computations* [^dennis-van-horn-1966] (publisher paywall; free retrieval not possible on 2026-08-14): the origin of the capability concept, the ability as an unforgeable artifact of authorization; F1 and F3 stand in this tradition.
* Hardy, *The Confused Deputy* [^hardy-confused-deputy] (reprint, checked 2026-08-14): the classic argument for why rights should hang on the artifact instead of on the caller; the background of the confused-deputy row in the security considerations and of the binding in F7.
* Birgisson et al., *Macaroons* [^macaroons]: the reference model for capability attenuation, a question that is not yet settled in the scope.
* OWASP Gen AI Security Project, *LLM01:2025 Prompt Injection* [^owasp-llm01]: the independent classification of the attack path against which the never-rules (F10) and the injection row of the security considerations are directed.

[^dennis-van-horn-1966]: https://doi.org/10.1145/365230.365252
[^hardy-confused-deputy]: https://css.csail.mit.edu/6.858/2015/readings/confused-deputy.html
[^macaroons]: https://research.google/pubs/macaroons-cookies-with-contextual-caveats-for-decentralized-authorization-in-the-cloud/
[^owasp-llm01]: https://genai.owasp.org/llmrisk/llm01-prompt-injection/
