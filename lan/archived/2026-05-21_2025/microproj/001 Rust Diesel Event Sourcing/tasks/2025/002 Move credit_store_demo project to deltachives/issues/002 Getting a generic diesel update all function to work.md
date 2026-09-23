---
parent: '[[002 Move credit_store_demo project to deltachives]]'
spawned_by: '[[002 Move credit_store_demo project to deltachives]]'
context_type: issue
status: skipped
---

Parent: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned by: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned in: [^spawn-issue-861a54](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md#spawn-issue-861a54)

# 1 Journal

2025-09-19 Wk 38 Fri - 19:45 +03:00

Automation for Insert was much simpler:

````rust
pub trait InsertHeadRec {
    type NewRec;
    type Rec;

    fn insert_head_rec(
        conn: &mut SqliteConnection,
        table: impl Table,
        object: Self::NewRec,
        tx_ea_msg: Sender<EaThreadMessage>,
    ) -> Result<Self::Rec, HeadRecOpError> {
        let out = diesel::insert_into(table)
            .values(&object)
            .returning(CreditStoreHeadRec::as_returning())
            .get_result(conn)?;

        tx_ea_msg.send(EaThreadMessage::DbWrite {
            table_name: object.get_table_group_id(),
        })?;

        Ok(out)
    }
}
````

2025-09-19 Wk 38 Fri - 20:25 +03:00

Or maybe not so simple since it gives us this error

````rust
pub mod autogen;
^^^
````

````
error[E0275]: overflow evaluating the requirement `diesel::expression::operators::Eq<_, &_>: diesel::Insertable<_>`
  |
  = help: consider increasing the recursion limit by adding a `#![recursion_limit = "256"]` attribute to your crate (`credit_store_demo`)
  = note: required for `&diesel::expression::operators::Eq<_, _>` to implement `diesel::Insertable<_>`
  = note: 126 redundant requirements hidden
  = note: required for `&Option<Option<Option<Option<Option<Option<Option<Option<...>>>>>>>>` to implement `diesel::Insertable<_>`
  = note: the full name for the type has been written to '/home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/target/debug/deps/credit_store_demo-4fa9e0a64f9f4ad8.long-type-14634543152730437809.txt'
  = note: consider using `--verbose` to print the full type name to the console

````

2025-09-19 Wk 38 Fri - 20:26 +03:00

This happens for both the update and the insert generic traits.
