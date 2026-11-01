---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[001 Create coin table events to experiment with aggregation being in views]]'
context_type: task
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [001 Create coin table events to experiment with aggregation being in views](001%20Create%20coin%20table%20events%20to%20experiment%20with%20aggregation%20being%20in%20views.md)

Spawned in: [^spawn-task-48bbe6](001%20Create%20coin%20table%20events%20to%20experiment%20with%20aggregation%20being%20in%20views.md#spawn-task-48bbe6)

# 1 Journal

2025-10-02 Wk 40 Thu - 10:57 +03:00

````sql
-- in migrations/2025-09-25-225000_create_coin_store/down.sql
DROP TABLE coin_store_diffs;
DROP TABLE coin_store_events;
DROP VIEW v_coin_store_events_grouped;
DROP VIEW v_coin_store_objects;
````

`up.sql` is the same as the [latest](../investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md#recall-3909fc) in [002 Investigate group by logic for frame and span to include up to span](../investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md)

Then as in the README.md,

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
source ./.env && rm $DATABASE_URL; diesel migration run && python3 scripts/diesel-postprocess.py

# out (error, relevant)
2025-10-02T08:02:22.352887Z ERROR diesel::database: Failed to execute query query
...
.save \"events.db\"\n" err=DatabaseError(Unknown, "near \".\": syntax error"
````

Remove `.save "events.db"`

And it's `OK`.

2025-10-02 Wk 40 Thu - 11:07 +03:00

The first thing we notice is that in `schema.rs` diesel only recognizes the tables, even though `data/database.db` does have the views applied.

Diesel does not have built-in support for reading our views. See [gh diesel-rs/diesel #4077](https://github.com/diesel-rs/diesel/pull/4077) for recent ongoing work on this as of this writing.

2025-10-02 Wk 40 Thu - 11:32 +03:00

They have a partial [feature/view_support branch](https://github.com/weiznich/diesel/tree/feature/view_support). Can we try it?

Spawn [003 Attempt to use partial view support branch of diesel](003%20Attempt%20to%20use%20partial%20view%20support%20branch%20of%20diesel.md) ^spawn-task-91bae6

2025-10-02 Wk 40 Thu - 12:38 +03:00

It does not migrate views. Other options include using [`SqlQuery -> sql_query`](https://github.com/weiznich/diesel/blob/da578f2af39bdd7e433cd0c7ca3286c6da6af1fd/diesel/src/query_builder/functions.rs#L614)

2025-10-02 Wk 40 Thu - 13:00 +03:00

From [000 Investigate summing and latest aggregation with diesel > ^recall-b48bf5](../investigations/000%20Investigate%20summing%20and%20latest%20aggregation%20with%20diesel.md#recall-b48bf5),

 > 
 > We're able to create custom SQL queries with [diesel guides extending-diesel](https://diesel.rs/guides/extending-diesel.html). This might be good to define event $\to$ aggregates seamlessly.

2025-10-02 Wk 40 Thu - 13:07 +03:00

As they show in [diesel.rs](https://diesel.rs/)

They have an example for `sql_query` in the [website markdown](https://github.com/sgrif/diesel.rs-website/blob/25a2a888112ccf9f9467d9294f726b0d82fd9c48/src/index.md?plain=1#L422).

2025-10-02 Wk 40 Thu - 13:19 +03:00

But how do we then go on about filtering or querying the views?

It seems we will need to use raw SQL while using them for now rather than the query builder. [`SqlQuery -> sql_query`](https://github.com/weiznich/diesel/blob/da578f2af39bdd7e433cd0c7ca3286c6da6af1fd/diesel/src/query_builder/functions.rs#L614) did mention

2025-10-03 Wk 40 Fri - 03:24 +03:00

Spawn [000 To materialize grouped events and accumulated objects into tables via software](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md) ^spawn-jdgmt-733656
