---
context_type: entry
---

Parent: [lan/2026/topic/concept/000 Atomic/wiki/003 Wiki Clusterline Concepts/003 Wiki Clusterline Concepts](../003%20Wiki%20Clusterline%20Concepts.md)

Spawned by: [lan/2026/topic/concept/000 Atomic/wikiproc/003 Wiki Proc Clusterline Concepts/003 Wiki Proc Clusterline Concepts](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md)

Spawned in: [^spawn-entry-6756bc](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/003%20Wiki%20Proc%20Clusterline%20Concepts.md#spawn-entry-6756bc)

Process Note: [004 Proc Use timestamp subcards and markers like TODO to allow multiple active threads within the same process note](../../../wikiproc/003%20Wiki%20Proc%20Clusterline%20Concepts/entry/004%20Proc%20Use%20timestamp%20subcards%20and%20markers%20like%20TODO%20to%20allow%20multiple%20active%20threads%20within%20the%20same%20process%20note.md)

---

Often it is good to maintain [atomicity](../concept/004%20Atomicity%20of%20Context.md) in our process notes, so that it is scoped for a singular concern rather than many.

This division of labor can happen before the work, when we already have a clear idea of a subgoal for example. And it can happen after the work, when retrospectively a part of the process note is clear in the end that it was a subgoal of the larger pursuit.

Sometimes we may be involved in multiple threads which are fairly coupled to our pursuit. In this case, we can make use of [timestamp subcards](024%20Timestamps%20in%20journals%20produce%20cards%20and%20subcards%20that%20segment%20the%20note%20into%20progressive%20points%20of%20attention.md) tied to a card and then with a marker like `TODO` added to it that signals this card needs to be pursued further. These markers can be grepped, so this can also allow threads active across multiple note files.

Timestamp cards can also be imbued with metadata themselves for this sort of decentralized inboxing, for example `t:idea[t:_] My Idea Title`. They can be given status, too, like `t:status=todo` within the `t:cat` (`t:idea`, `t:task`, ...)

The metadata should be used for more long-term threads. This is particularly useful in case of recording ideas, reminders, or concerns that the current pursuit does not want to dedicate time to. They can be grepped later to be resolved, and can be given a topic of interest with `t:topic={mytopic}` and a subtopic if needed `t:subtopic={mysubtopic}`, or otherwise just tags `t:tags={comma-separated-tags}`

The markers can be used for more temporary threads that should be resolved soon, although they can remain if they are in `PEND` status, as this is expected to accumulate freely while `TODO` should be only for the active threads.

Use the `OK` marker to signal a thread ends. I often use this at the end of `# Resolution` or a journal, to end the main thread and resolve the process note.

In software, often markers are used like `FIXME` and others for the same purpose. Instead of maintaining a central inbox of reminders, they are structurally tied to various places in the source code. This is a similar idea. We can navigate the notes using markers and metadata on subcards to provide a decentralized inbox of subcards.

Since [Timestamp cards segment our note into progressive points of attention](024%20Timestamps%20in%20journals%20produce%20cards%20and%20subcards%20that%20segment%20the%20note%20into%20progressive%20points%20of%20attention.md), it can be helpful to start a card with a clear purpose. For example a single investigative question, or a thing to try. Then the card can clearly designate the thread that we may want to attempt simultaneously with others when we search the markers.
