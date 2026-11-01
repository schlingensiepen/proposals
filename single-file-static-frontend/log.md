# Update log

## 2026-11-01
* **Update**: Form (agent guide name, README describes the parts, relation vocabulary).

## 2026-10-13
* **Update**: Bundle form. Same content as version 1.0, rearranged into the files of an OKF bundle: rules in `rules/`, scope, evidence, sources and terms in files of their own, the agent guide with a self-check. The open questions of 1.0 moved to "not yet settled" in the scope. US spelling; wording smoothed. Version stays 1.0.

## 2026-08-15
* **Creation**: Version 1.0, first version. Eight rules: one artifact without a build step (F1), unauthenticated delivery and therefore no data in the document (F2), no application code in the delivery path (F3), the address as a state of the client rather than a resource of the server (F4), that state in the fragment so the server need not be bent (F5), vendored rather than linked third-party assets (F6), files around the API envelope (F7, recommendation), one origin for delivery and API (F8, recommendation). The core of the cut is simple deployment while keeping structural security; the hosting restriction "no URL rewriting" is an example, not the cause. Six threats in the security considerations; no reference implementation yet, thought through, not proven; four open questions.
