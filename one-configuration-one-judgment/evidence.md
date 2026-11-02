---
type: Proposal Evidence
description: Where the concept has held; its origin in the series, the empty-value incident in a container deployment, and the recurring second truth of separate validation tools.
---

# Evidence

## Origin

The concept was split out of `service-as-single-artifact` when it became
clear that several proposals in the series need it
without needing the rest of that design. The judgment rule and the duty
to name the source have been part of it since its first version (August
2026).

## The empty value

The case behind [W2](./choices/W2-the-empty-value.md) has occurred. In a
container deployment by the author a secret reference was set through
an environment variable whose value to substitute was missing on the
host; the platform set the variable to empty. The service at first did
not distinguish "not set" from "set and empty"; troubleshooting came down
to the question of which of two places was in effect. The fix was the
distinction W2 demands, and the abort with a reason, option (b). The
repository concerned is not publicly accessible.

## The second truth

The case behind [F1](./rules/F1-the-judgment-is-the-start.md) is
documented several times in the series: a validation tool that reads
differently from the service, and a service that does not start although
the check was green. The series has done without separate validation
tools since `service-as-single-artifact` was written.
