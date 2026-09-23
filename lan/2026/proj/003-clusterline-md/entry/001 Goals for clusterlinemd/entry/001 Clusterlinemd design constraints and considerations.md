---
context_type: entry
---

Parent: [lan/2026/proj/003-clusterline-md/entry/001 Goals for clusterlinemd/001 Goals for clusterlinemd](../001%20Goals%20for%20clusterlinemd.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/001 Goals for clusterlinemd/001 Goals for clusterlinemd](../001%20Goals%20for%20clusterlinemd.md)

Spawned in: [^spawn-entry-f45b6b](../001%20Goals%20for%20clusterlinemd.md#spawn-entry-f45b6b)

---

# Constraint

1. Note handles mean that renaming is more fragile. Renaming a note named `X` requires that all notes `Handle X` also be renamed.

# Decision

 > 
 > 1. Note handles mean that renaming is more fragile. Renaming a note named `X` requires that all notes `Handle X` also be renamed.

Some ways we can handle this:

1. Ask the user to not use the default rename function but `Clusterline: rename`.
   * upside: Action does what it says.
   * downside: User now has multiple ways to rename which behave differently; confusing.
1. Keep the default rename behavior, expect the user to use `Clusterline: fix` as a catch-all that will also rename these notes based on metadata.
   * upside: Can catch other issues too.
   * upside: Even if not selected as the workflow, probably good to have anyway.
   * downside: Sounds like an exceptional activity. The user won't have reason to run this if they expect the repository to be in a good state. Effectively, we are *forcing* the user to put their repo in a broken state and then offering them a solution for it!
1. Find a way to listen to a rename event, and complete the user's job by renaming the handles too.
   * upside: There is an editor-standard way to rename, and the repetitive part is dealt with once the user gives a clear rename signal.
   * upside: Even if they rename the handle, we can rename the original for them too!
   * downside: Requires the editor to expose a rename file event. Do many do this?

(2) is unacceptable due to expecting clusterline normal workflow to result in any broken repo state. So try (3), and if it fails. do (1).

# Journal
