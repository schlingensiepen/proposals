---
type: Proposal Rule
description: The commands configure the function for the specific case and create the access for exactly that configuration in one act; secrets of a variant are persisted in the store, never referenced.
number: F9
binding: must
role: load-bearing
applies_to: [builder, operator]
imports: [where-a-secret-comes-from]
---

# F9 · Configuration and access come into being in one act

**Applies to:** builder · operator.

**What holds.** The commands configure the function **for the specific use case** — which target system, which storage location, which limits — and create the access for **exactly this configuration**; both are one motion. What the configuration determines, the caller never chooses: the access carries it. There is no access without a matching configuration, and a change that shifts the variant (the target, the storage location) is a new access; a change that leaves it (limits, the credentials of the target system) is what the change command is for. Secrets that belong to a variant (the password of the target system) the setup command takes at invocation over the ranks of `where-a-secret-comes-from`, never as a plain-text argument, and persists them in the store (F12): as a hash where the service only verifies (salted where a human chose the secret); in plain text under tight permissions only where it must present them. They are not referenced there.

**Why.** Here the permission question disappears before it arises. Configuring is granting; what has been granted is in the configuration; the access is merely the handle on it. There is no point in time at which someone *adds* something to an existing access — and hence nothing to administer either (F10). Incidentally, one check in the service disappears: the caller cannot name the target at all, so nobody has to check whether they would be allowed to. **This holds expressly for single-operator use as well**, where operator and caller are the same person and an access artifact seems superfluous at first: it costs nothing there, because it arises along with the configuration anyway — there is no step one could save. Retrofitting, by contrast, is expensive: then existing callers have to be converted, targets have to be bound to accesses after the fact, and a variant has to be invented where everything used to be global — and experience shows that this happens exactly when the second user arrives and there is no time. Built in from the start, the design scales upward without ever having to be rebuilt: the step from the first caller to the second is then a single command invocation.

**Considered and rejected.** *First create the access, then configure* — creates the intermediate state "access without a variant", and it is exactly in this in-between space that permission management arises. *Making the target selectable at runtime ("the caller says where")* — shifts the responsibility to the caller and forces the service into a check it would not need without this choice. *Self-registration with subsequent release* — sensible for services that want to take agents on without the operator lifting a finger (see the sources); unnecessary here, because the operator configures anyway, and expensive, because it introduces exactly that intermediate state.

**How you notice it is violated.** One is discussing which permissions an existing access should get.
