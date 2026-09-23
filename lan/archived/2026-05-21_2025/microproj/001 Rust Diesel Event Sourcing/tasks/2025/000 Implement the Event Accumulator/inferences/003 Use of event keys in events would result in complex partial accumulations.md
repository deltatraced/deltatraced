---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[001 To disallow event keys for joins but require materialized table unique ids for object in history]]'
context_type: inference
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [001 To disallow event keys for joins but require materialized table unique ids for object in history](../judgments/001%20To%20disallow%20event%20keys%20for%20joins%20but%20require%20materialized%20table%20unique%20ids%20for%20object%20in%20history.md)

Spawned in: [^spawn-infer-c52a0d](../judgments/001%20To%20disallow%20event%20keys%20for%20joins%20but%20require%20materialized%20table%20unique%20ids%20for%20object%20in%20history.md#spawn-infer-c52a0d)

# 1 Related

This depends on having made the judgment [000 To materialize grouped events and accumulated objects into tables via software](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md)

# 2 Journal

2025-10-03 Wk 40 Fri - 05:08 +03:00

From [004 Choosing accumulations or events as keys in events](../tasks/004%20Choosing%20accumulations%20or%20events%20as%20keys%20in%20events.md),

If events could contain keys to other events, support we have an event table with $N$ keys to join on. An accumulation will now show $N$ event ids in it, if we were to join on them, it would be 1 accumulated and $N$ event data, which would need to be accumulated on their own. This is complex to handle and does not scale well with our current approach of creating an accumulation view per event table.

For more info on the current approach with event views, see [002 Investigate group by logic for frame and span to include up to span](../investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md).
