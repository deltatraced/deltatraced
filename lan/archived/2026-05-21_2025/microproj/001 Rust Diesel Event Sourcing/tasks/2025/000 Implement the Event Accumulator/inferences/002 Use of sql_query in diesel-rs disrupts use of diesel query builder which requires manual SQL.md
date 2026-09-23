---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 To materialize grouped events and accumulated objects into tables via software]]'
context_type: inference
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 To materialize grouped events and accumulated objects into tables via software](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md)

Spawned in: [^spawn-infer-b130c8](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md#spawn-infer-b130c8)

# 1 Journal

2025-10-03 Wk 40 Fri - 03:55 +03:00

We learned that diesel-rs does not yet support views in [000 diesel-rs does not yet support views](000%20diesel-rs%20does%20not%20yet%20support%20views.md).

From [002 Add event accumulation events through diesel](../tasks/002%20Add%20event%20accumulation%20events%20through%20diesel.md),

Use of [sql_query](https://github.com/sgrif/diesel.rs-website/blob/25a2a888112ccf9f9467d9294f726b0d82fd9c48/src/index.md?plain=1#L422) would allow us to read views by extending diesel, but at the cost that all these queries will have to be written in raw SQL without assistance of the query builder.

There are many ways to build a query. Filters, grouping, joins, etc. that would now be written without type safety.

[sql_query](https://github.com/sgrif/diesel.rs-website/blob/25a2a888112ccf9f9467d9294f726b0d82fd9c48/src/index.md?plain=1#L422) may be best used for single instances rather than a general means of access to a table. Otherwise, it may also be used to retrieve entire results that can then be filtered via rust iterators.
