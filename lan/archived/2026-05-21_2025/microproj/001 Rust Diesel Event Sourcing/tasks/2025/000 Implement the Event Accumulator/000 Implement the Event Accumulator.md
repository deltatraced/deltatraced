---
status: todo
---

# 1 Journal

2025-09-20 Wk 38 Sat - 20:46 +03:00

Spawn [000 Allow users to append new credit store events in various ways](tasks/000%20Allow%20users%20to%20append%20new%20credit%20store%20events%20in%20various%20ways.md) ^spawn-task-b96c13

2025-09-21 Wk 38 Sun - 20:29 +03:00

We need to revise how event sourcing is implemented. Using complex instructions for the event accumulator like `undo N` and `redo N` would make it extremely difficult to query events. Similar for global/local events. Instead of popping frames, we can simply add local events which are where actual objects are touched, at a specific scale no and frame no. Frame no is so that even though we may add events at the same scale, we start from scratch (relative to that scale) on a new frame. It's up to software how events at different scales interact. Whether higher scales aggregate into lower scales, or whether lower scales are treated as more permanent effects that are applied to higher scales.

We might not need a Head managed by an event accumulator and a Version to check out with, if we can simply get the state of affairs by filter by scale, and frame no.  We should look into use of query projections to get the state of current objects. We might also need to leave it to software what updating means. For numerical values it may be differential, so that the column needs to aggregate. For others, we may need to replace the column value by the latest. Another thing is how deletions affect

2025-09-21 Wk 38 Sun - 21:01 +03:00

Might be best to separate local and global events. Local being changes to objects, and global being changes to the entire state of affairs.

Objects should have a status that can be aggregated. Inserted, Updated, Deleted. An Insert event followed by updates, gives us Updated. An Insert event alone gives us Inserted, and if it ends with a Delete, we get Deleted. This could be satisfied with a sum aggregate with the following rules:

1 is inserted. 0 is deleted. $\gt$ 1 is updated. On insert, the value is set to 1. On each update, it's incremented once, and finally on a delete, the number of events added for that object are counted, and then they are subtracted, so $-N$ to get us to 0. Now a sum aggregate will work to give us what objects currently exist.

^recall-244d1b

2025-09-21 Wk 38 Sun - 21:34 +03:00

Spawn [000 Investigate summing and latest aggregation with diesel](investigations/000%20Investigate%20summing%20and%20latest%20aggregation%20with%20diesel.md) ^spawn-invst-bb50da

Spawn [001 Use of views and CTEs with sqlite3 and diesel-rs](investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md) ^spawn-invst-3617a0

2025-09-21 Wk 38 Sun - 23:07 +03:00

Spawn [000 Resources encountered during event accumulator impl](entries/000%20Resources%20encountered%20during%20event%20accumulator%20impl.md) ^spawn-entry-4b539d

2025-09-21 Wk 38 Sun - 23:21 +03:00

Spawn [001 Create coin table events to experiment with aggregation being in views](tasks/001%20Create%20coin%20table%20events%20to%20experiment%20with%20aggregation%20being%20in%20views.md) ^spawn-task-9b8a2b

2025-10-14 Wk 42 Tue - 18:33 +03:00

Spawn [005 Model coin_store_hist and related in diesel](tasks/005%20Model%20coin_store_hist%20and%20related%20in%20diesel.md) ^spawn-task-276e67

2025-11-22 Wk 47 Sat - 16:27 +03:00

Spawn [009 Fix conceptual bug of not tracking branch information for soft reset](tasks/009%20Fix%20conceptual%20bug%20of%20not%20tracking%20branch%20information%20for%20soft%20reset.md) ^spawn-task-0f0f75

# 2 Spawn Trees

* [000 Implement the Event Accumulator](000%20Implement%20the%20Event%20Accumulator.md)
  * entry [000 Resources encountered during event accumulator impl](entries/000%20Resources%20encountered%20during%20event%20accumulator%20impl.md)
  * rejected investigation [000 Investigate summing and latest aggregation with diesel](investigations/000%20Investigate%20summing%20and%20latest%20aggregation%20with%20diesel.md)
  * investigation [001 Use of views and CTEs with sqlite3 and diesel-rs](investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md)
    * howto [001 Creating a basic counter with a recursive CTE in sqlite3](howtos/001%20Creating%20a%20basic%20counter%20with%20a%20recursive%20CTE%20in%20sqlite3.md)
    * howto [002 Creating a basic table duplicator with recursive CTE in sqlite3](howtos/002%20Creating%20a%20basic%20table%20duplicator%20with%20recursive%20CTE%20in%20sqlite3.md)
  * todo task [000 Allow users to append new credit store events in various ways](tasks/000%20Allow%20users%20to%20append%20new%20credit%20store%20events%20in%20various%20ways.md)
  * task [001 Create coin table events to experiment with aggregation being in views](tasks/001%20Create%20coin%20table%20events%20to%20experiment%20with%20aggregation%20being%20in%20views.md)
    * investigation [002 Investigate group by logic for frame and span to include up to span](investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md)
      * investigation [003 Create a natural numbers table and group by divisibility up to N](investigations/003%20Create%20a%20natural%20numbers%20table%20and%20group%20by%20divisibility%20up%20to%20N.md)
        * howto [000 Create sqlite3 dbs from sql script](howtos/000%20Create%20sqlite3%20dbs%20from%20sql%20script.md)
    * task [002 Add event accumulation events through diesel](tasks/002%20Add%20event%20accumulation%20events%20through%20diesel.md)
      * judgment [000 To materialize grouped events and accumulated objects into tables via software](judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md)
        * inference [000 diesel-rs does not yet support views](inferences/000%20diesel-rs%20does%20not%20yet%20support%20views.md)
        * inference [002 Use of sql_query in diesel-rs disrupts use of diesel query builder which requires manual SQL](inferences/002%20Use%20of%20sql_query%20in%20diesel-rs%20disrupts%20use%20of%20diesel%20query%20builder%20which%20requires%20manual%20SQL.md)
        * inference [001 Event loads can contain complex join structures](inferences/001%20Event%20loads%20can%20contain%20complex%20join%20structures.md)
          * task [004 Choosing accumulations or events as keys in events](tasks/004%20Choosing%20accumulations%20or%20events%20as%20keys%20in%20events.md)
            * judgment [001 To disallow event keys for joins but require materialized table unique ids for object in history](judgments/001%20To%20disallow%20event%20keys%20for%20joins%20but%20require%20materialized%20table%20unique%20ids%20for%20object%20in%20history.md)
              * inference [003 Use of event keys in events would result in complex partial accumulations](inferences/003%20Use%20of%20event%20keys%20in%20events%20would%20result%20in%20complex%20partial%20accumulations.md)
              * inference [004 IDs in events must be to a unique object, which cannot be object id for a historic table](inferences/004%20IDs%20in%20events%20must%20be%20to%20a%20unique%20object,%20which%20cannot%20be%20object%20id%20for%20a%20historic%20table.md)
        * investigation [004 Investigate options for materializing views into tables using SQL](investigations/004%20Investigate%20options%20for%20materializing%20views%20into%20tables%20using%20SQL.md)
      * task [003 Attempt to use partial view support branch of diesel](tasks/003%20Attempt%20to%20use%20partial%20view%20support%20branch%20of%20diesel.md)
  * task [005 Model coin_store_hist and related in diesel](tasks/005%20Model%20coin_store_hist%20and%20related%20in%20diesel.md)
    * task [006 Using macro_rules to automate event and hist creation](tasks/006%20Using%20macro_rules%20to%20automate%20event%20and%20hist%20creation.md)
      * entry [001 Reading through lukaswirth.dev decl-macros](entries/001%20Reading%20through%20lukaswirth.dev%20decl-macros.md)
    * task [007 Remove obselete credit store code for old software managed event sourcing](tasks/007%20Remove%20obselete%20credit%20store%20code%20for%20old%20software%20managed%20event%20sourcing.md)
    * task [008 Add model validation for person and coin writes](tasks/008%20Add%20model%20validation%20for%20person%20and%20coin%20writes.md)

# 3 Index

**entry**

[000 Resources encountered during event accumulator impl](entries/000%20Resources%20encountered%20during%20event%20accumulator%20impl.md)

[001 Reading through lukaswirth.dev decl-macros](entries/001%20Reading%20through%20lukaswirth.dev%20decl-macros.md)

**howto**

[000 Create sqlite3 dbs from sql script](howtos/000%20Create%20sqlite3%20dbs%20from%20sql%20script.md)

[001 Creating a basic counter with a recursive CTE in sqlite3](howtos/001%20Creating%20a%20basic%20counter%20with%20a%20recursive%20CTE%20in%20sqlite3.md)

[002 Creating a basic table duplicator with recursive CTE in sqlite3](howtos/002%20Creating%20a%20basic%20table%20duplicator%20with%20recursive%20CTE%20in%20sqlite3.md)

**inference**

[000 diesel-rs does not yet support views](inferences/000%20diesel-rs%20does%20not%20yet%20support%20views.md)

[002 Use of sql_query in diesel-rs disrupts use of diesel query builder which requires manual SQL](inferences/002%20Use%20of%20sql_query%20in%20diesel-rs%20disrupts%20use%20of%20diesel%20query%20builder%20which%20requires%20manual%20SQL.md)

[001 Event loads can contain complex join structures](inferences/001%20Event%20loads%20can%20contain%20complex%20join%20structures.md)

[003 Use of event keys in events would result in complex partial accumulations](inferences/003%20Use%20of%20event%20keys%20in%20events%20would%20result%20in%20complex%20partial%20accumulations.md)

[004 IDs in events must be to a unique object, which cannot be object id for a historic table](inferences/004%20IDs%20in%20events%20must%20be%20to%20a%20unique%20object,%20which%20cannot%20be%20object%20id%20for%20a%20historic%20table.md)

**investigation**

rejected [000 Investigate summing and latest aggregation with diesel](investigations/000%20Investigate%20summing%20and%20latest%20aggregation%20with%20diesel.md)

[001 Use of views and CTEs with sqlite3 and diesel-rs](investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md)

[002 Investigate group by logic for frame and span to include up to span](investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md)

[003 Create a natural numbers table and group by divisibility up to N](investigations/003%20Create%20a%20natural%20numbers%20table%20and%20group%20by%20divisibility%20up%20to%20N.md)

[004 Investigate options for materializing views into tables using SQL](investigations/004%20Investigate%20options%20for%20materializing%20views%20into%20tables%20using%20SQL.md)

**judgment**

[000 To materialize grouped events and accumulated objects into tables via software](judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md)

[001 To disallow event keys for joins but require materialized table unique ids for object in history](judgments/001%20To%20disallow%20event%20keys%20for%20joins%20but%20require%20materialized%20table%20unique%20ids%20for%20object%20in%20history.md)

**task**

todo [000 Allow users to append new credit store events in various ways](tasks/000%20Allow%20users%20to%20append%20new%20credit%20store%20events%20in%20various%20ways.md)

[001 Create coin table events to experiment with aggregation being in views](tasks/001%20Create%20coin%20table%20events%20to%20experiment%20with%20aggregation%20being%20in%20views.md)

[002 Add event accumulation events through diesel](tasks/002%20Add%20event%20accumulation%20events%20through%20diesel.md)

[003 Attempt to use partial view support branch of diesel](tasks/003%20Attempt%20to%20use%20partial%20view%20support%20branch%20of%20diesel.md)

[004 Choosing accumulations or events as keys in events](tasks/004%20Choosing%20accumulations%20or%20events%20as%20keys%20in%20events.md)

[005 Model coin_store_hist and related in diesel](tasks/005%20Model%20coin_store_hist%20and%20related%20in%20diesel.md)

[006 Using macro_rules to automate event and hist creation](tasks/006%20Using%20macro_rules%20to%20automate%20event%20and%20hist%20creation.md)

[007 Remove obselete credit store code for old software managed event sourcing](tasks/007%20Remove%20obselete%20credit%20store%20code%20for%20old%20software%20managed%20event%20sourcing.md)

[008 Add model validation for person and coin writes](tasks/008%20Add%20model%20validation%20for%20person%20and%20coin%20writes.md)
