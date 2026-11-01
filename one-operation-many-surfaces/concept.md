---
type: Proposal Concept
description: The problem of a service whose operations exist once per surface and drift apart unnoticed, the concept of one operation per action with every surface a pass-through onto it, checked structurally, its load-bearing structure, and the three-step procedure.
---

# The concept

## Problem

The builder who gives his service a second surface beside the command suspects that the second surface creates two implementations of the same operation.

In the author's implementation the administrative operations initially existed twice, once for the command line, once for HTTP, and a later comparison found **ten** places where they had drifted apart: one surface reported more on a failure than the other, one knew a server feature the other did not read, one turned every unknown status into a server error, one had one status line fewer. Each of the two foundations for the target system had to be hardened twice, two bugs found there fixed twice each; one description still advertised behavior the other side had long removed. Since then the operation exists once, and a structural test holds that (F1, F2).

## The concept

A service has a second surface beside its command, or further ones. Underneath lies **one** operation per administrative action (create an account, issue an access, withdraw it, check the store, clean up the queue), and both surfaces are **pass-throughs** onto it: they read arguments, call the operation, translate its result into their vocabulary (status code here, exit code there) and contain no logic of their own. Because two implementations of the same operation each work on their own, this property is **checked structurally**: a test parses the modules of both pass-throughs and fails where an operation is performed instead of called.

**Load-bearing structure.** F1 and F2 belong together and are the condition under which one may have two surfaces at all: without the one operation there are two administrations drifting apart; without the structural test the one operation is an intention that falls at the first wish for a helper function; if F2 falls, nobody knows whether F1 still holds.

## The procedure

The rules say what must hold. This section says in which order one proceeds.

**1 · Secure the operations.** Make sure every administrative action exists as a callable function apart from any command and any handler, raising its own error kinds. If the logic lives in the commands today, extract it first. (F1)

**2 · Write the structural tests before the first handler.** Whitelist of imports, list of operation names, file system ban with named exceptions, "every command and handler calls an operation". Run each once with its protection removed. (F2)

**3 · Build the routes as pass-throughs.** Mirror every command. (F1)
