---
type: Proposal Concept
description: The problem of delivering a system of several parts as a secrets problem rather than a transfer problem, the concept of one script per component and one directory per instance, its load-bearing structure, and the procedure in six steps.
---

# The concept

## Problem

"Automated deployment" is usually solved as a transfer problem: push the files over, done. As soon as a system consists of several parts and needs access control, however, it is a **secrets problem**: there are accesses to the foreign machines that are delivered to, and accesses that only **come into being** during delivery. Whoever separates the two cleanly and lets neither of them into version control gets the order of the parts for free. The price is a second directory per instance that nobody backs up except the operator.

"My system consists of three parts: an application on rented web space, a background process on a machine at my place, a backup on a third one. Each is delivered differently. Each needs credentials to get there. And the background process needs a key for the application that does not even exist beforehand. Where do I put all of that without it ending up in the repository, and in which order do I do it?"

This contains **two kinds of secrets** that are regularly mixed up:

| | Example | Property |
|---|---|---|
| **Access to the host system** | SSH key, password for the file transfer | exists **beforehand**, belongs to the operator, is only **used** |
| **Secrets of the system** | signing key, deploy key, machine accesses | comes into being **during delivery**, belongs to the system, is **created and put in place** |

The first kind must never go into the repository. Neither may the second, and from it follows in addition the **order** in which the parts have to be delivered. That is the connection one only sees when looking at both together.

## The concept

For each **component** there is a script, and for each **instance** a directory. The script is versioned and knows no target; the directory lies outside version control and knows exactly one: it carries the address, the credentials and all settings of this instance. The script is invoked with the directory as its only parameter.

What is transferred is **a list from version control**, not the contents of a directory. Runtime data is locked in the core of the script, not exempted by a configuration line. The script creates the production configuration if it is missing and **never** overwrites it, except on an explicit instruction that names what it destroys.

The script creates the secrets of the system itself and puts them beside the instance configuration. Accesses for other components come into being in a **time-limited window** that the delivery opens and that closes by itself. From this follows the order: whoever can only get their secret while the window is open is delivered after the one who opens it.

Above everything stands an entry point that runs, creates and lists targets. And every kind of delivery comes with a guide to doing the same **by hand**.

**Load-bearing structure.** **F1 carries everything**: without the separation of script and instance there is no place where secrets would be allowed to lie. **F2 and F6 together produce F7**: only when the script owns the system secrets *and* they come into being only in a window does the order follow from the matter itself instead of from an agreement; if either falls, F7 becomes a convention again. **F3, F4 and F5 protect the store** and can be adopted individually; F5 is the most dangerous of them, because its `--force`, executed wrongly, does exactly the damage that F4 prevents. F8 is convenience with a side effect close to security: a tool that knows the order is not operated wrongly. F9 and F10 stand independently. **Whoever takes only one rule takes F1.**

## The procedure

The rules say what must hold. This section orders them in the sequence in which a delivery passes through them; it adds nothing to them.

**1 · Name the components and instances.** Which parts are delivered independently, onto which host systems? *Result:* one script per component, one directory per instance outside version control; the ignore pattern covers all instance directories. (F1)

**2 · Sort the secrets.** For each secret: access to the host system (exists beforehand, lies in the instance directory) or system secret (comes into being during delivery, created by the script). For each access that one component needs for another: who opens the window, who receives. *Result:* the list of secrets and, from it, the order of the first delivery. (F2, F6, F7)

**3 · Build the scripts.** Package contents from the list of versioned files, protective files explicitly versioned; runtime data locked in the core; create the production configuration, never overwrite it, `--force` names what it destroys before it acts; create the system secrets, place them, back them up beside the instance configuration and name them in the terminal; open the window; check from outside. Beside each script lies its versioned sample configuration. (F2 to F6, F9)

**4 · Build the entry point.** Run in the enforced order; create a target (directory with narrow permissions, template copied in, nothing transferred); list what exists. (F7, F8)

**5 · Write the guides for manual operation.** One per component and transfer path, first installation and migration of the store included. (F10)

**6 · First run, second run.** The first run per instance creates secrets, creates configurations and follows the order; every later run only transfers files, and the order no longer matters. (F5, F7)
