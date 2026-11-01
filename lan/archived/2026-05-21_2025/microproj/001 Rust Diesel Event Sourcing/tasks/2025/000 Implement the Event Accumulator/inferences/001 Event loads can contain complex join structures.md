---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 To materialize grouped events and accumulated objects into tables via software]]'
context_type: inference
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 To materialize grouped events and accumulated objects into tables via software](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md)

Spawned in: [^spawn-infer-bcaf57](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md#spawn-infer-bcaf57)

# 1 Inference

In [001 Create coin table events to experiment with aggregation being in views](../tasks/001%20Create%20coin%20table%20events%20to%20experiment%20with%20aggregation%20being%20in%20views.md) and [002 Investigate group by logic for frame and span to include up to span](../investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md),

We created an event accumulation view that takes into account sourcing of events from lower spans at frame creation time.

This is complex, however, the test was made with a simple table with no join structure or any other features. It is possible for the load diff to compose with other tables, which will also need to be events if they can change or can remain as objects if they're append-only.

~~Events may compose with other events, but accumulations should only compose with accumulations.~~

As per judgment [001 To disallow event keys for joins but require materialized table unique ids for object in history](../judgments/001%20To%20disallow%20event%20keys%20for%20joins%20but%20require%20materialized%20table%20unique%20ids%20for%20object%20in%20history.md),

Both events and accumulations will only hold keys to historic or ahistoric objects, to simplify joins and decouple them from the dimension of accumulation.

# 2 Journal

2025-10-03 Wk 40 Fri - 03:41 +03:00

Spawn [004 Choosing accumulations or events as keys in events](../tasks/004%20Choosing%20accumulations%20or%20events%20as%20keys%20in%20events.md) ^spawn-task-9c15b1
