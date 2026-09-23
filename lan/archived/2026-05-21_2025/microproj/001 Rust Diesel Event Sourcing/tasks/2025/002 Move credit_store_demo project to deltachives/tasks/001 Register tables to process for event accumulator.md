---
parent: '[[002 Move credit_store_demo project to deltachives]]'
spawned_by: '[[002 Move credit_store_demo project to deltachives]]'
context_type: task
status: done
---

Parent: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned by: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned in: [^spawn-task-0a20b1](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md#spawn-task-0a20b1)

# 1 Journal

2025-09-19 Wk 38 Fri - 21:50 +03:00

In order to generalize and separate the managed tables from the event accumulator implementation, we need to be able to provide a way to register which tables to work on.

We need to know the `x_head`, `x_events`, `x`, and `x_version` tables.

2025-09-19 Wk 38 Fri - 22:25 +03:00

````rust
/// These capabilities allow us to manage a table head from its events
pub trait EventSourcable: 
    ReadEventTableVersion
    + WriteEventTableVersion
    + ReadEventTable
    + InsertIntoHeadTable
    + UpdateHeadTableRow
    + DeleteHeadTableRow
    + ClearHeadTable
    + SourceHeadTable
    { }
````

Now we can get a hash map of event sourcable objects. Let's implement this for the credit store.

2025-09-19 Wk 38 Fri - 22:35 +03:00

We really most of the time need to read just the newly appended events. `ReadEventTable` need to be able to support this.

2025-09-19 Wk 38 Fri - 23:32 +03:00

We need to update the schema since we made it so that the event id is required for version tables

Spawn [002 Update credit store schema for required version event id](002%20Update%20credit%20store%20schema%20for%20required%20version%20event%20id.md) ^spawn-task-bcd457

2025-09-20 Wk 38 Sat - 02:26 +03:00

Hmm, we currently had a new `CreditStoreObject` that doesn't derive anything diesel for use as the shared portion inside an event table. Another way was to use `NewCreditStoreRec`, but it would require some more lifetime management to all the traits using the object due to use of `&'a str`.

2025-09-20 Wk 38 Sat - 02:35 +03:00

We should be clear that, if an event id is provided to `read_event_table`, that it filters for ids $\gt$ it and not $\ge$ it. This is because this will represent the last processed id, so we don't ant to process it again.

2025-09-20 Wk 38 Sat - 02:38 +03:00

We also need to figure out how to ensure `Object` is shared across the traits...

^recall-8017b0

2025-09-20 Wk 38 Sat - 03:13 +03:00

We created previously a `delete_credit_store_head_rec`, but this shouldn't exist outside the trait impls. It shouldn't signal a write event either, because only the event accumulator can write to head tables.

2025-09-20 Wk 38 Sat - 03:42 +03:00

Now to trigger work to start, we need an ability for the user to insert an event. Events are part of an append-only store, so the user can only read or insert. In addition, we need to be able to send a message to tell the event accumulator to "check out" a new version of a head table to a given event sourcable key (i.e. the table name key string used for the event sourcable object map).

We added this to the `EaThreadMessage`:

````rust
/// On None, the event accumulator attempts to check out latest.
DbCheckout { table_name: String, opt_event_id: Option<i32>, },
````

This is a new message dedicated for checking out different versions of tables, so that the writing responsibility remains solely in the event accumulator's control.

2025-09-20 Wk 38 Sat - 03:56 +03:00

We need to add `&self` to all the traits to access them through the event sourcable object.

2025-09-20 Wk 38 Sat - 04:16 +03:00

Finally we reached the issue we [anticipated](001%20Register%20tables%20to%20process%20for%20event%20accumulator.md#recall-8017b0)!

````rust
let events = event_sourcable.read_event_table(&mut mut_conn, None)?;

let (event_data, obj) = events[0];

event_sourcable.update_head_table_row(&mut mut_conn, 1, obj)?;
````

````
error[E0308]: mismatched types
   --> src/db/event_accumulator.rs:168:81
    |
168 |                         event_sourcable.update_head_table_row(&mut mut_conn, 1, obj)?;
    |                                         ---------------------                   ^^^ expected `UpdateHeadTableRow::Object`, found `ReadEventTable::Object`
    |                                         |
    |                                         arguments to this method are incorrect
    |
    = note: expected associated type `<Src as UpdateHeadTableRow>::Object`
               found associated type `<Src as ReadEventTable>::Object`
    = note: an associated type was expected, but a different one was found
note: method defined here
   --> src/db/event_accumulator.rs:81:8
    |
81  |     fn update_head_table_row(&self, mu_conn: &mut SqliteConnection, object_id: i32, object: Self::Object) -> Result<(), EventSourcableErr...
    |        ^^^^^^^^^^^^^^^^^^^^^                                                        ------
````

2025-09-20 Wk 38 Sat - 04:25 +03:00

We solved this issue by adding a `HasEventObject` trait, and setting it as a constraint for any trait that needs `Object`. So we're not repeating `Object` anywhere else, and because multiple traits satisfy this constraint, we know they refer to the same `Object`, `<EventSourcable as HasEventObject>::Object`! We also added clone to the object and debug for ease of inspection and copying.

````
pub trait HasEventObject {
    /// The part of the event table that is duplicate of the base object
    type Object: Clone + Debug;
}
````

So now this works:

````rust
let events = event_sourcable.read_event_table(&mut mut_conn, None)?;

let (event_data, obj) = events[0].clone();

event_sourcable.update_head_table_row(&mut mut_conn, 1, obj)?;
````

Okay! Registeration seems good!

We will need to test much of this still, but we need to implement the event accumulation algorithm.
