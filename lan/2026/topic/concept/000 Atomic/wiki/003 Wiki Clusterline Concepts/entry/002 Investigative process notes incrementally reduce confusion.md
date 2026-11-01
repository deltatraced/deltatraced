---
context_type: entry
---

Parent: [lan/2026/topic/concept/000 Atomic/wiki/003 Wiki Clusterline Concepts/003 Wiki Clusterline Concepts](../003%20Wiki%20Clusterline%20Concepts.md)

Spawned by: [lan/2026/topic/concept/000 Atomic/wikiproc/003 Wiki Proc Clusterline Concepts/003 Wiki Proc Clusterline Concepts](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md)

Spawned in: [^spawn-entry-5bdf6a](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md#spawn-entry-5bdf6a)

Proc: [000 Proc Investigative process notes incrementally reduce confusion](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/entry/000%20Proc%20Investigative%20process%20notes%20incrementally%20reduce%20confusion.md)

---

A [process note](../concept/000%20Term%20Process%20Note.md) of the context type `investigation` can fulfill the role of a question asked at a given stage of understanding. The question can indicate the writer's mental model of the given domain.

A good investigation note is completed through reduction of uncertainty and confusion. Since [001 Process notes should be immutable to act as an audit](001%20Process%20notes%20should%20be%20immutable%20to%20act%20as%20an%20audit.md), the state of confusion is valuable to indicate, as the audit then records a path through confusion, with successive investigative notes being more specific questions that utilize more specialized language.

As an example, when looking to work on contributing work for an open source project, we may start at a high level of uncertainty as we have not studied the code base. Our initial question may track an aspect of our desired goal (an issue the PR is supposed to close). Successive questions would become more concerned with technical aspects of the codebase relevant to our desired goal.

Per [003 Overview notes in a cluster clarify how that cluster ought be navigated](003%20Overview%20notes%20in%20a%20cluster%20clarify%20how%20that%20cluster%20ought%20be%20navigated.md), it is useful to provide a link in PRs to an overview note that organizes the cluster of an issue. One key thing to display is the progression of investigative questions posed and solved, finally followed by any tasks required to finish the engineering task that closes the issue.

Make use of [005 Process Notes may contain artifact sections in non journal headers](005%20Process%20Notes%20may%20contain%20artifact%20sections%20in%20non%20journal%20headers.md). Since the overview section is linked in a PR, these wiki sections can give readers key information. Remember, [006 Process notes are meant to be mined for evidence rather than linearly read](006%20Process%20notes%20are%20meant%20to%20be%20mined%20for%20evidence%20rather%20than%20linearly%20read.md).

Also, remember these are notes, and so are by definition optional to check. If linked in a PR, do not give them high significance. A footnote reference about internal dev notes at the end can suffice.

# Incremental reduction in confusion

In a list, each investigative question should reduce confusion in some local sense. It may increase confusion in that it produces more unknowns and surprises, but should still resolve some prior confusion, if only just to express it in clearer terms in terms of newer confusions.

# Examples

## Corresponding feature request to code implementation

As an example, while I was doing a PR to silverbullet, I created an Overview handle note for the effort:

[001 Overview SB Option to only have base name in title](../../../../../oss/000%20OSS%20Contrib/task/000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title/entry/001%20Overview%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

This shows investigations that increase in technical specificity. First, I needed to know how the vary the title bar text at all in the source code. I also needed to know how the source code allows us to add new configuration to make it an opt-in feature. Once I understood how to do this in principal, then I needed to do it in a way that fits with the rest of the code base, hence the next investigations are about the workings of the plug system and interactions between synchronous and asynchronous code in HTML rendering. Finally after some investigations at increasingly technical levels from the intended feature, I was confident to start working on the implementation, and the overview shows the implementation note.
