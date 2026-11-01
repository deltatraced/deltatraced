---
context_type: entry
---

Parent: [lan/2026/proj/003-clusterline-md/entry/001 Goals for clusterlinemd/001 Goals for clusterlinemd](../001%20Goals%20for%20clusterlinemd.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/001 Goals for clusterlinemd/001 Goals for clusterlinemd](../001%20Goals%20for%20clusterlinemd.md)

Spawned in: [^spawn-entry-1b08bd](../001%20Goals%20for%20clusterlinemd.md#spawn-entry-1b08bd)

---

# Version 0.1 Initial Functionality

We some core functionality to be able to interact with the filesystem schema of clusterlinemd.

Features, sorted by high priority for use:

* [ ] Add a command to navigate to all subnotes, and one to all main notes.
  
  * [ ] [006 Clusterlinemd Impl open main note and sub note](../../000-clusterline-md-getting-started/task/006%20Clusterlinemd%20Impl%20open%20main%20note%20and%20sub%20note.md)
* [ ] Ask for a user, and a project category, spawn a project.

* [ ] Ask for a user, and a project category, spawn a document.

* [ ] Ask for a project, spawn a cluster.

* [ ] Spawn notes within a cluster

* [ ] Detect when the cursor is on an aspiring note `[[note]]`, and use the name when user asks to spawn a note

* [ ] Ask for a project, spawn both a wiki cluster and a dual process cluster, have them reference one another.

* [ ] Add a command to automatically fix non-complete spawn note lines under cursor from obsidian

* [ ] Autogenerate `project-index` for all projects in the space, ‘cluster-index`, for all clusters in the space, and `note-index\` for all notes in the space, all alphabetically sorted

* [ ] For each cluster, create the following in an `autogen/` folder, where `X` is the number of the cluster: `000 X Context Index`, `001 X Spawn Tree`.

* [ ] Add a command to paste and move images to attachment/ after paste

* [ ] Add a command to fix spawned by relations that need resync after note refactoring (move of corresponding Spawn note)

* [ ] If a subnote  with the name `entry/Overview {X}` where X is the cluster name exists, link to it when spawning new notes in X

* [ ] Add ability to spawn via a note handle. For example an `Overview X` for some cluster `X`, or `Iterations for X` for some note `X`.

* [ ] Add ability to spawn from a cluster, which automatically logs the spawn in a note handle `Spawns for X` where `X` is the cluster name.

* [x] Add a command to put the current timestamp

* [x] Add a command to make note urls space relative (make_note_link_absolute 0e36536)

* [x] Add a command to copy the link of the current page in a format directly used in `[[]]`

# Version 0.2

# Later

# See Also

* [001 Clusterlinemd design constraints and considerations](001%20Clusterlinemd%20design%20constraints%20and%20considerations.md)
