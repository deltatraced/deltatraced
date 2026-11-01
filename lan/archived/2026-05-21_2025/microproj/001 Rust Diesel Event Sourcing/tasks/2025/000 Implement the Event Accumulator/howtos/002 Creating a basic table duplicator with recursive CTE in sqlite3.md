---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[001 Use of views and CTEs with sqlite3 and diesel-rs]]'
context_type: howto
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [001 Use of views and CTEs with sqlite3 and diesel-rs](../investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md)

Spawned in: [^spawn-howto-1d4875](../investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md#spawn-howto-1d4875)

# 1 Objective

In sqlite3,

Given some table, duplicate its rows $N$ times, and append an extra column for each row identifying the nth duplicate that it is.

# 2 Related

* [001 Creating a basic counter with a recursive CTE in sqlite3](001%20Creating%20a%20basic%20counter%20with%20a%20recursive%20CTE%20in%20sqlite3.md)
* [003 Create a natural numbers table and group by divisibility up to N](../investigations/003%20Create%20a%20natural%20numbers%20table%20and%20group%20by%20divisibility%20up%20to%20N.md)

# 3 Journal

2025-09-26 Wk 39 Fri - 23:34 +03:00

````sql
-- in duplicator.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  max INTEGER NOT NULL
);

INSERT INTO constants (max) VALUES 
	(5);

CREATE TABLE fruits (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  name TEXT NOT NULL,
  color TEXT NOT NULL,
  cost INTEGER NOT NULL
);

INSERT INTO fruits (name, color, cost) VALUES
	('strawberry', 'red', 10),
	('banana', 'yellow', 9);

WITH RECURSIVE duplicator(dup, name, color, cost) AS (
  SELECT 0, name, color, cost
    FROM fruits
  UNION
  SELECT dup + 1, name, color, cost
    FROM duplicator
    WHERE dup < (SELECT max FROM constants)
)
SELECT * FROM duplicator;
````

````sh
cat duplicator.sql | sqlite3

# out
0|strawberry|red|10
0|banana|yellow|9
1|strawberry|red|10
1|banana|yellow|9
2|strawberry|red|10
2|banana|yellow|9
3|strawberry|red|10
3|banana|yellow|9
4|strawberry|red|10
4|banana|yellow|9
5|strawberry|red|10
5|banana|yellow|9
````

2025-09-26 Wk 39 Fri - 23:44 +03:00

Unsure how to remove the dependency to specify table columns

2025-09-27 Wk 39 Sat - 01:28 +03:00

Instead of just using naturals for `dep`, we can also substitute in other tables while using `dep` for their nth entry (this [post](https://www.geeksforgeeks.org/sqlite/how-to-select-the-nth-row-in-a-sqlite-database-table/) explains getting the nth row from a table).

2025-09-27 Wk 39 Sat - 02:07 +03:00

This [stackoverflow answer](https://stackoverflow.com/a/24862165/6944447) explores using joins to pass columns to inner queries.

2025-09-27 Wk 39 Sat - 02:12 +03:00

With this we're able to join `dup` and `id` of `alphabets`, which could be any table that we want to act as the duplication load.

````sql
-- in duplicator.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  max INTEGER NOT NULL
);

INSERT INTO constants (max) VALUES 
	(5);

CREATE TABLE fruits (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  name TEXT NOT NULL,
  color TEXT NOT NULL,
  cost INTEGER NOT NULL
);

INSERT INTO fruits (name, color, cost) VALUES
	('strawberry', 'red', 10),
	('banana', 'yellow', 9)
;

CREATE TABLE alphabet (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  lower TEXT NOT NULL,
  upper TEXT NOT NULL
);

INSERT INTO alphabet (lower, upper) VALUES
	('a', 'A'),
	('b', 'B'),
	('c', 'C'),
	('d', 'D'),
	('e', 'E')
;

WITH RECURSIVE duplicator(dup, name, color, cost) AS (
  SELECT 0, name, color, cost
    FROM fruits
  UNION
  SELECT dup + 1, name, color, cost
    FROM duplicator
    WHERE dup < (SELECT max FROM constants)
)
SELECT 
	*
FROM duplicator t2
JOIN alphabet t1 ON t2.dup = t1.id
;
````

````sh
cat duplicator.sql | sqlite3

# out
1|strawberry|red|10|1|a|A
1|banana|yellow|9|1|a|A
2|strawberry|red|10|2|b|B
2|banana|yellow|9|2|b|B
3|strawberry|red|10|3|c|C
3|banana|yellow|9|3|c|C
4|strawberry|red|10|4|d|D
4|banana|yellow|9|4|d|D
5|strawberry|red|10|5|e|E
5|banana|yellow|9|5|e|E
````

2025-09-27 Wk 39 Sat - 02:18 +03:00

In case `id` does not match `dup` for some reason but we expect the tables to be of the same size, we can use a windowing function to generate an index and then join on that index

````sql
--- in duplicator.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  max INTEGER NOT NULL
);

INSERT INTO constants (max) VALUES 
	(5);

CREATE TABLE fruits (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  name TEXT NOT NULL,
  color TEXT NOT NULL,
  cost INTEGER NOT NULL
);

INSERT INTO fruits (name, color, cost) VALUES
	('strawberry', 'red', 10),
	('banana', 'yellow', 9)
;

CREATE TABLE alphabet (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  lower TEXT NOT NULL,
  upper TEXT NOT NULL
);

INSERT INTO alphabet (lower, upper) VALUES
	('a', 'A'),
	('b', 'B'),
	('c', 'C'),
	('d', 'D'),
	('e', 'E')
;

WITH RECURSIVE duplicator(dup, name, color, cost) AS (
  SELECT 0, name, color, cost
    FROM fruits
  UNION
  SELECT dup + 1, name, color, cost
    FROM duplicator
    WHERE dup < (SELECT max FROM constants)
)
SELECT 
	*
FROM duplicator t1
JOIN 
	(SELECT ROW_NUMBER() OVER () AS rownumber, * FROM alphabet) t2 
	ON t1.dup = t2.rownumber
````

````sh
cat duplicator.sql | sqlite3

# out
1|strawberry|red|10|1|1|a|A
1|banana|yellow|9|1|1|a|A
2|strawberry|red|10|2|2|b|B
2|banana|yellow|9|2|2|b|B
3|strawberry|red|10|3|3|c|C
3|banana|yellow|9|3|3|c|C
4|strawberry|red|10|4|4|d|D
4|banana|yellow|9|4|4|d|D
5|strawberry|red|10|5|5|e|E
5|banana|yellow|9|5|5|e|E
````
