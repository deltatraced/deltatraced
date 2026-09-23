---
context_type: entry
---

Parent: [lan/2026/topic/concept/000 Atomic/wiki/003 Wiki Clusterline Concepts/003 Wiki Clusterline Concepts](../003%20Wiki%20Clusterline%20Concepts.md)

Spawned by: [lan/2026/topic/concept/000 Atomic/wikiproc/003 Wiki Proc Clusterline Concepts/003 Wiki Proc Clusterline Concepts](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md)

Spawned in: [^spawn-entry-a0e76f](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md#spawn-entry-a0e76f)

---

# Benefits of schematized filesystems

A schema-less filesystem imposes many decisions on the note user. Where should we put a note? How should we group notes? One may opt for emergant organization via linking alone and act on a flat list, but the emergent links may not always be optimal for navigation.

# Specifics of clusterline as an example

For the case of clusterline,

the note filesystem is a deterministic schema. In the notes root, one may have many users. `lan/` is my user, you may add another, we may have a `shared/` space of collaboration in writing.

Within `/{user}/` we have  `/{user}/YYYY` and `/{user}/archived`. This decision was made so that we get a fresh space every year. We can of course continue working and linking to what was already started in prior years. Inevitably, systems change, and we want to archive things. We track this ahead of time under `/{user}/archived/{archive-id}` and `/{user}/archived/archive-reason` explains why.

Within a year or an archive, we have a collection of clusters (individual note graphs) that we call `clusterspace`s.

clusterspaces have types that determine how they are to be interpreted. For example:

* `/proj/000 My Proj` is a clusterspace of type project. It is a folder dedicated to all the work of that project.
* `/microproj/001 My little experiment` is a clusterspace of type `microproj` which is a small project, often for demos, experiments, and other explanatory and exploratory purposes.

There is the special clusterspace `main`, which comes without a type: `/main`. This is the user-wide clusterspace.

There is also a special clusterspace `/topic/{my topic}` where `{my topic}` may be set to any user-specified grouping of clusterspaces. For example:

* `/topic/book-notes/{Author clusterspace}/`: `/topic/book-notes` allows us to dedicate a place for all author clusterspaces, which themselves explore various works of the author in question.
* `/topic/oss/003 Open Source Proj/` to track open source projects under group `oss`.

Next we have cluster types and clusters. Just like clusterspace types and clusterspaces, the types tell us how to interpret the cluster. Some clusters are of type `task`, so they are actionable. Others of type `entry`, which is useful for organization and open-ended exploration. Some are of type `issue`, which can index code forge issues like from a github project. The clusters themselves contain a core note with their same name, so: `/topic/oss/003 Open Source Proj/task/000 Build SilverBulletmd/000 Build Silvrebulletmd`.

Once we have a cluster, we can start spawning peripheral notes that identify it as their center. The notes too have types which help us interpret them. All in all, we may have the investigation `lan/2026/topic/tut/000 Explain Filesystem Schema/investigation/000 What makes a good example?`.

At the levels of the clusterspace and the cluster we may add `st/{status}` before them. This can help to deal with clutter if we have a lot of content. Status can include `done`, `mightdo`, `idea`, `wontdo`.

clusterspaces, clusters, and subnotes all begin with a triplet number `NNN` which helps giving them a stable point of reference. We can refer to them by number, and they are more stable in case of title renames.

It would be tedious to maintain all this manually, so we ought to use some tooling that respects this schema. This is the purpose of the `clusterline-md` set of tools: [001 Goals for clusterlinemd](../../../../../../proj/003-clusterline-md/entry/001%20Goals%20for%20clusterlinemd/001%20Goals%20for%20clusterlinemd.md)

**in Summary,**

We have a fixed schema that allows us to add new groups of clusterspaces, which have clusters in them, which have peripheral notes in them, all given a specific type to interpret how to work with them. clusterspaces, clusters, and peripheral notes all begin with a counting index: typically a triplet. These are the basic grouping units to be handled with tools for where to place a note.

# Guiding principles for clusterline

No arbitrary hierarchies. There are fundamentally 3-4 hierarchical levels. One more if we are defining a project group, but besides that, it is roughly group, cluster, note. The idea is that we should organize these so that it is easy to "append one more". One more note can be spawned arbitrarily in a cluster. One more cluster can be added easily like key tasks in a project, and one more project can be added easily as well which we may take advantage of some project group for them. In other words, we are not addressing a position in the context of a project like `Table Tennis Practice > Serving Practice > Research > Some Source Notes`. Instead we aim to represent these as flat lists of different levels. We may have a clusterspace `000 Tennis Practice`, and then a flat list of clusters (which act as our original aim or intent) like `task/000 Research routines to improve aim in table tennis`, `task/001 Setup a way to record consistency in table tennis serving`, and so on. Then when the cluster is created, we spawn whatever notes are consequently needed by them. We end up creating a chronologically ordered set of clusters on what we are doing at the moment rather than trying to somehow figure out how we could provide a semantic tree for table tennis practice to fit notes in. They always just fit at the end. This can lower cognitive load as there aren't many decisions we have to make as to where to put a new note.
