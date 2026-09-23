---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[003 Create a natural numbers table and group by divisibility up to N]]'
context_type: howto
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [003 Create a natural numbers table and group by divisibility up to N](../investigations/003%20Create%20a%20natural%20numbers%20table%20and%20group%20by%20divisibility%20up%20to%20N.md)

Spawned in: [^spawn-howto-c2d6b4](../investigations/003%20Create%20a%20natural%20numbers%20table%20and%20group%20by%20divisibility%20up%20to%20N.md#spawn-howto-c2d6b4)

# 1 Journal

2025-09-26 Wk 39 Fri - 08:01 +03:00

This [stackoverflow answer](https://stackoverflow.com/a/69123002/6944447) shows that we can pipe sql script directly to sqlite3.

But when I tried nothing happened:

````sh
cat main.sql          

# out
CREATE TABLE nat (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  value INTEGER NOT NULL UNIQUE
);
````

````sh
cat main.sql | sqlite3
````

But we do get output if our `main.sql` is

````
CREATE TABLE nat (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  value INTEGER NOT NULL UNIQUE
);

.save "main.db"
````
