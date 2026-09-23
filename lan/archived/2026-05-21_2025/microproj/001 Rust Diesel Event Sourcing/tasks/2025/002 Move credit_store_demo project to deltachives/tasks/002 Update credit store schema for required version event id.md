---
parent: '[[002 Move credit_store_demo project to deltachives]]'
spawned_by: '[[001 Register tables to process for event accumulator]]'
context_type: task
status: skipped
---

Parent: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned by: [001 Register tables to process for event accumulator](001%20Register%20tables%20to%20process%20for%20event%20accumulator.md)

Spawned in: [^spawn-task-bcd457](001%20Register%20tables%20to%20process%20for%20event%20accumulator.md#spawn-task-bcd457)

# 1 Journal

2025-09-19 Wk 38 Fri - 23:33 +03:00

In models, we had to change it to not be optional:

````diff
#[derive(Insertable, AsChangeset)]
#[diesel(treat_none_as_null = true)]
#[diesel(table_name = crate::autogen::schema::credit_store_version)]
pub struct NewCreditStoreVersion {
-    pub optevent_id: Option<i32>,
+    pub event_id: i32,
}
````

as well in CreditStoreVersion.

Since there's so made types just for the concept of credit store we might want to put them in a separate module, but not currently.

2025-09-19 Wk 38 Fri - 23:38 +03:00

As mentioned in the README,

Let's regenerate the schema:

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
source ./.env && rm $DATABASE_URL; diesel migration run && python3 scripts/diesel-postprocess.py

# out 
rm: cannot remove 'data/database.db': No such file or directory
Running migration 2025-09-12-162639_create_credit_store
````

Seems to have ran fine still.

2025-09-19 Wk 38 Fri - 23:43 +03:00

Need to update `actions.rs` also. Actions here are what the user can do but is not managed by the event accumulator. Those are implemented as actions for event sourcable objects passed in.

Users shouldn't write to the version table directly, they should instruct the event accumulator to change versions.

2025-09-20 Wk 38 Sat - 01:37 +03:00

Hmm... Even though there should be an event version, it's possible for there to be no events. In that case, what would the version be? This might be a case to revert these changes and keep it as optional.

We need to also remember to initialize the version table if a record doesn't exist. By default, it should be None if no events exist, or the latest event id.

2025-09-20 Wk 38 Sat - 01:42 +03:00

Reverting back to `opt_event_id`. We need to handle there possibly being no events. Also, even if there were events, and yet none were yet processed by the event accumulator, it should remain `None`.  This table is managed only by the event accumulator, so it always knows how many events were processed.

2025-09-20 Wk 38 Sat - 01:46 +03:00

Moving all version related operations out of `actions.rs` as the user shouldn't use them.

2025-09-20 Wk 38 Sat - 02:16 +03:00

Making all the models also derive `Debug` so they're easy to log and inspect.
