---
status: done
---

# 1 Journal

2025-09-18 Wk 38 Thu - 18:09 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs

````

2025-09-18 Wk 38 Thu - 19:46 +03:00

We need to reorganize how we spawn a shell. We can have many experiments which require the same shell infrastructure but with different commands.

Spawn [000 Modularize shi shell use in credit store demo](tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md) ^spawn-task-dfb856

2025-09-19 Wk 38 Fri - 19:32 +03:00

Spawn [002 Getting a generic diesel update all function to work](issues/002%20Getting%20a%20generic%20diesel%20update%20all%20function%20to%20work.md) ^spawn-issue-861a54

2025-09-19 Wk 38 Fri - 20:27 +03:00

Complex to automate through types, so we might have to look for other methods.

2025-09-19 Wk 38 Fri - 21:50 +03:00

Spawn [001 Register tables to process for event accumulator](tasks/001%20Register%20tables%20to%20process%20for%20event%20accumulator.md) ^spawn-task-0a20b1

2025-09-20 Wk 38 Sat - 04:38 +03:00

We still need to be able to write events in an append-only fashion, and signal work to the event accumulator, but this now looks to be in a good state. So let's try to create the repository.

2025-09-20 Wk 38 Sat - 04:45 +03:00

Handling some lints, which include:

* module level docs not having `//!` and instead having `///`.
* prefer to impl `Display` instead of `ToString`.
* use `Vec::new` instead of `|| vec![]`

2025-09-20 Wk 38 Sat - 04:51 +03:00

All lints passed!

# 2 Spawn Trees

* [002 Move credit_store_demo project to deltachives](002%20Move%20credit_store_demo%20project%20to%20deltachives.md)
  * skipped issue [002 Getting a generic diesel update all function to work](issues/002%20Getting%20a%20generic%20diesel%20update%20all%20function%20to%20work.md)
  * task [000 Modularize shi shell use in credit store demo](tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md)
    * investigation [000 Reviewing impl Trait type meaning](investigations/000%20Reviewing%20impl%20Trait%20type%20meaning.md)
    * issue [000 Internal state for shi shell not passing to new thread safely](issues/000%20Internal%20state%20for%20shi%20shell%20not%20passing%20to%20new%20thread%20safely.md)
    * issue [001 Getting many debugging logs from rustyline while using shi](issues/001%20Getting%20many%20debugging%20logs%20from%20rustyline%20while%20using%20shi.md)
      * idea [000 Can eliminate all logging calls with cargo feature](ideas/000%20Can%20eliminate%20all%20logging%20calls%20with%20cargo%20feature.md)
  * task [001 Register tables to process for event accumulator](tasks/001%20Register%20tables%20to%20process%20for%20event%20accumulator.md)
    * skipped task [002 Update credit store schema for required version event id](tasks/002%20Update%20credit%20store%20schema%20for%20required%20version%20event%20id.md)

# 3 Index

**idea**

[000 Can eliminate all logging calls with cargo feature](ideas/000%20Can%20eliminate%20all%20logging%20calls%20with%20cargo%20feature.md)

**investigation**

[000 Reviewing impl Trait type meaning](investigations/000%20Reviewing%20impl%20Trait%20type%20meaning.md)

**issue**

[000 Internal state for shi shell not passing to new thread safely](issues/000%20Internal%20state%20for%20shi%20shell%20not%20passing%20to%20new%20thread%20safely.md)

[001 Getting many debugging logs from rustyline while using shi](issues/001%20Getting%20many%20debugging%20logs%20from%20rustyline%20while%20using%20shi.md)

skipped [002 Getting a generic diesel update all function to work](issues/002%20Getting%20a%20generic%20diesel%20update%20all%20function%20to%20work.md)

**task**

[000 Modularize shi shell use in credit store demo](tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md)

[001 Register tables to process for event accumulator](tasks/001%20Register%20tables%20to%20process%20for%20event%20accumulator.md)

skipped [002 Update credit store schema for required version event id](tasks/002%20Update%20credit%20store%20schema%20for%20required%20version%20event%20id.md)
