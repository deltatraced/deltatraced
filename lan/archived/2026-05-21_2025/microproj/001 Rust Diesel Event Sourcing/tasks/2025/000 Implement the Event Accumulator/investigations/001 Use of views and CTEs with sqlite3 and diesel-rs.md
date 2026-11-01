---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 Implement the Event Accumulator]]'
context_type: investigation
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned in: [^spawn-invst-3617a0](../000%20Implement%20the%20Event%20Accumulator.md#spawn-invst-3617a0)

# 1 Objective

Since events are differential and hard to view directly, we would like to see if we can create an aggregate view in the sqlite3 database.

Secondarily, we would like to figure out general group bys to be used in the views that go beyond categorical data.

# 2 Related

[001 Create coin table events to experiment with aggregation being in views](../tasks/001%20Create%20coin%20table%20events%20to%20experiment%20with%20aggregation%20being%20in%20views.md)

# 3 Journal

2025-09-21 Wk 38 Sun - 22:39 +03:00

I wrote projection initially, but I think we're looking for views here. Projections seem to do more with distinct selections (as explored in [stackoverflow answer](https://stackoverflow.com/questions/3461099/what-is-a-projection)).

There's information about the [sqlite architecture](https://sqlite.org/arch.html) and [sqlite overview](https://sqlite.org/howitworks.html).

2025-09-21 Wk 38 Sun - 22:53 +03:00

They have information specifically on creating Sqlite views in [sqlite createview](https://sqlite.org/lang_createview.html).

2025-09-26 Wk 39 Fri - 03:19 +03:00

They maintain an alphabetical list of all documents for sqlite3 in [sqlite.org doclist](https://sqlite.org/doclist.html).

They also have [virtual tables](https://sqlite.org/lang_createvtab.html), which for example our Head managed table would fit the definition of, although with the limitation that it would not store anything and require software, so it might be best to use managed tables since they will remain in the database file.

2025-09-26 Wk 39 Fri - 03:27 +03:00

There's a [tutorial](https://www.sqlitetutorial.net/sqlite-create-view/) on creating view in sqlite. It seems they use the notation `v_xxx` for views, so let's do this too. Although other tutorials do not do this.

2025-09-26 Wk 39 Fri - 03:38 +03:00

There's also a [tutorial](https://www.sqlitetutorial.net/sqlite-group-by/) on group by. We're interested in `SUM` for transactions and differentials and `MAX` for things like timestamps. They have a [tutorial](https://www.sqlitetutorial.net/sqlite-aggregate-functions/) on some aggregates, but it's not clear if this is exhaustive. Here is the [reference aggregate functions](https://sqlite.org/lang_aggfunc.html).

2025-09-26 Wk 39 Fri - 09:12 +03:00

This is another [groupby tutorial](https://www.wscubetech.com/resources/sql/group-by) with some different simple use cases.

2025-09-26 Wk 39 Fri - 03:49 +03:00

This [stackoverflow post](https://stackoverflow.com/questions/10999522/how-to-get-the-latest-record-in-each-group-using-group-by) has some hints on finding the last. Some suggestions include another query that gets the max of id to do it.

2025-09-26 Wk 39 Fri - 08:42 +03:00

This [sqldocs.org sqlite views post](https://sqldocs.org/sqlite-database/sqlite-views/)  has notes on the conditions of mutable views. It seems Sqlite3 allows this for simple views based on a single table, but not mutli-table views.

2025-09-26 Wk 39 Fri - 09:43 +03:00

The group bys we want are non-categorical. This [stackoverflow answer](https://stackoverflow.com/a/38821165/6944447) hints to use of `WITH RECURSIVE` as in [sqlite.org with-clause](https://sqlite.org/lang_with.html).

2025-09-26 Wk 39 Fri - 19:03 +03:00

[SQL UNION ALL tutorial](https://www.w3schools.com/sql//sql_union_all.asp)

There is a [tutorial](https://www.sqlservertutorial.net/sql-server-basics/sql-server-recursive-cte/) for recursive CTEs. This is a useful template for a recursive CTE from there

````sql
WITH expression_name (column_list)
AS
(
    -- Anchor member
    initial_query  
    UNION ALL
    -- Recursive member that references expression_name.
    recursive_query  
)
-- references expression name
SELECT *
FROM   expression_name
````

But their basic example doesn't work out of the box in sqlite3.

Spawn [001 Creating a basic counter with a recursive CTE in sqlite3](../howtos/001%20Creating%20a%20basic%20counter%20with%20a%20recursive%20CTE%20in%20sqlite3.md) ^spawn-howto-d079c0

2025-09-26 Wk 39 Fri - 21:18 +03:00

Spawn [002 Creating a basic table duplicator with recursive CTE in sqlite3](../howtos/002%20Creating%20a%20basic%20table%20duplicator%20with%20recursive%20CTE%20in%20sqlite3.md) ^spawn-howto-1d4875
