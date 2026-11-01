---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 Implement the Event Accumulator]]'
context_type: task
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned in: [^spawn-task-276e67](../000%20Implement%20the%20Event%20Accumulator.md#spawn-task-276e67)

# 1 Journal

2025-10-14 Wk 42 Tue - 18:34 +03:00

We've never automated creating the `down.sql`, only the `up.sql` with `*.dbmts`. Maybe we can derive a `down.sql` from an `up.sql`, since we just have to drop tables and views currently though.

Similar to [dbmint 003 Impl writing the expanded event sourcing output given the grouped settings by table for dbmts.md](https://github.com/dbmint/dbmint-notes/blob/7796b7d5f4e9e742ae931ee3ace4c060eb936b88/lan/tasks/2025/004%20Create%20dbmts%20format%20with%20event%20sourcing%20support/tasks/003%20Impl%20writing%20the%20expanded%20event%20sourcing%20output%20given%20the%20grouped%20settings%20by%20table%20for%20dbmts.md),

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store
~/src/cloned/gh/dbmint/dbmts_rs/target/debug/dbmts_rs to_sql <(cat << 'EOF'
#[derive(EventSourcing)]
CREATE TABLE coin_store_diffs (
  id INTEGER NOT NULL PRIMARY KEY,
  obj_id INTEGER NOT NULL,
  #[es(latest)]
  person TEXT NOT NULL,
  #[es(sum)]
  coins INTEGER NOT NULL
);
EOF
) > up.sql
````

Though let's keep the version before the processing:

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store
cat <(cat << 'EOF'
#[derive(EventSourcing)]
CREATE TABLE coin_store_diffs (
  id INTEGER NOT NULL PRIMARY KEY,
  obj_id INTEGER NOT NULL,
  #[es(latest)]
  person TEXT NOT NULL,
  #[es(sum)]
  coins INTEGER NOT NULL
);
EOF
) > up.dbmts

~/src/cloned/gh/dbmint/dbmts_rs/target/debug/dbmts_rs to_sql up.dbmts > up.sql
````

2025-10-14 Wk 42 Tue - 19:05 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store
cat up.sql \
	| grep CREATE \
	| sed 's/ ($//g' \
	| sed 's/ AS$//g' \
	| sed 's/CREATE/DROP/g' \
	| sed 's/$/;/g' > down.sql
````

````Rust
// in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/scripts/schema.rs.pre
#[derive(diesel_derive_enum::DbEnum, Debug, strum::VariantArray, Clone)]
pub enum EventAction {
    Insert,
    Update,
    Delete,
    Open,
    Close,
    Reopen,
}
````

As in the README,

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/
source ./.env && rm $DATABASE_URL; diesel migration run && python3 scripts/diesel-postprocess.py
````

Now we create the models in `model.rs`

2025-10-14 Wk 42 Tue - 19:30 +03:00

We need to modify `dbmts_rs` to make it autoincrement events, and we need to autoincrement the diffs ourselves.

2025-10-14 Wk 42 Tue - 19:40 +03:00

Spawn [006 Using macro_rules to automate event and hist creation](006%20Using%20macro_rules%20to%20automate%20event%20and%20hist%20creation.md) ^spawn-task-6cd569

2025-10-20 Wk 43 Mon - 22:00 +03:00

Spawn [007 Remove obselete credit store code for old software managed event sourcing](007%20Remove%20obselete%20credit%20store%20code%20for%20old%20software%20managed%20event%20sourcing.md) ^spawn-task-e6701d

2025-10-20 Wk 43 Mon - 22:27 +03:00

Spawn [008 Add model validation for person and coin writes](008%20Add%20model%20validation%20for%20person%20and%20coin%20writes.md) ^spawn-task-d31c69
