---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 Implement the Event Accumulator]]'
context_type: investigation
status: rejected
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned in: [^spawn-invst-bb50da](../000%20Implement%20the%20Event%20Accumulator.md#spawn-invst-bb50da)

# 1 Objective

We want to be able to sum some columns, update others by latest, and assume others are constant as in group keys.

We need to be able to demonstrate this as a diesel query aggregating events into an object.

# 2 Journal

2025-09-21 Wk 38 Sun - 21:36 +03:00

This [stackoverflow answer](https://stackoverflow.com/questions/72670161/how-do-you-use-rust-diesel-to-do-a-group-by-query) hints at column-level aggregation rules with select.

2025-09-21 Wk 38 Sun - 21:50 +03:00

[diesel docs](https://docs.diesel.rs/master/diesel/index.html).

2025-09-21 Wk 38 Sun - 22:20 +03:00

We're able to create custom SQL queries with [diesel guides extending-diesel](https://diesel.rs/guides/extending-diesel.html). This might be good to define event $\to$ aggregates seamlessly.

^recall-b48bf5
