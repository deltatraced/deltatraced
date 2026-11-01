---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[001 Use of views and CTEs with sqlite3 and diesel-rs]]'
context_type: howto
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [001 Use of views and CTEs with sqlite3 and diesel-rs](../investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md)

Spawned in: [^spawn-howto-d079c0](../investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md#spawn-howto-d079c0)

# 1 Objective

SQL has an advanced feature called `CTE` (Common Table Expression), which can be recursive with [sqlite.org with-clause](https://sqlite.org/lang_with.html) and RECURSIVE.

We want to be able to create an $N$-counter with it.

# 2 Related

* [002 Creating a basic table duplicator with recursive CTE in sqlite3](002%20Creating%20a%20basic%20table%20duplicator%20with%20recursive%20CTE%20in%20sqlite3.md)
* [003 Create a natural numbers table and group by divisibility up to N](../investigations/003%20Create%20a%20natural%20numbers%20table%20and%20group%20by%20divisibility%20up%20to%20N.md)

# 3 Journal

2025-09-26 Wk 39 Fri - 20:09 +03:00

This [post](https://www.sqlservercentral.com/articles/hidden-rbar-counting-with-recursive-ctes) has some example on counters but they also don't work out of the box in sqlite3.

2025-09-26 Wk 39 Fri - 20:28 +03:00

This [simonwillison.net post](https://til.simonwillison.net/sqlite/simple-recursive-cte) has a basic counter that does work out of the box in sqlite3:

````sql
-- in counter.sql
WITH RECURSIVE counter(x) AS (
  SELECT 0
    UNION
  SELECT x + 1 FROM counter
)
SELECT * FROM counter LIMIT 5;
````

````sh
cat counter.sql | sqlite3

# out
0
1
2
3
4
````

2025-09-26 Wk 39 Fri - 22:37 +03:00

We can also use this to get our limit from another query:

````sql
-- in counter.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  n INTEGER NOT NULL
);

INSERT INTO constants (n) VALUES 
	(5);

SELECT n FROM constants;
````

````sh
cat counter.sql | sqlite3

# out
5
````

2025-09-26 Wk 39 Fri - 22:52 +03:00

Here's the same concept but using a downcounter supplied with a max amount from another query:

````sql
--- in counter.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  max INTEGER NOT NULL
);

INSERT INTO constants (max) VALUES 
	(5);

WITH RECURSIVE downcounter(x) AS (
  SELECT max 
		FROM constants
  UNION
  SELECT x - 1 
		FROM downcounter
		WHERE x > 0
)
SELECT *
FROM downcounter;
````

````sh
cat counter.sql | sqlite3

# out
5
4
3
2
1
0
````

2025-09-26 Wk 39 Fri - 22:56 +03:00

This is the same concept, but with an up counter:

````sql
-- in counter.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  max INTEGER NOT NULL
);

INSERT INTO constants (max) VALUES 
	(5);

WITH RECURSIVE counter(x) AS (
  SELECT 0
  UNION
  SELECT x + 1 
		FROM counter
		WHERE x + 1 < (SELECT max FROM constants)
)
SELECT *
FROM counter;
````

````sh
cat counter.sql | sqlite3

# out
0
1
2
3
4
````

2025-09-26 Wk 39 Fri - 23:01 +03:00

Using the more basic example above, we can just use `LIMIT` and inject a query to calculate the limit in it.

````sql
-- in counter.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  max INTEGER NOT NULL
);

INSERT INTO constants (max) VALUES 
	(5);

WITH RECURSIVE counter(x) AS (
  SELECT 0
  UNION
  SELECT x + 1 FROM counter
)
SELECT * FROM counter LIMIT (SELECT max FROM constants);
````

````sh
cat counter.sql | sqlite3

# out
0
1
2
3
4
````

2025-09-27 Wk 39 Sat - 00:03 +03:00

This gives us a way of generating a `v_nat` view!

````sql
-- in counter.sql
CREATE VIEW v_nat
AS
	WITH RECURSIVE counter(n) AS (
		SELECT 0
		UNION
		SELECT n + 1 
			FROM counter
      LIMIT 5
	)
	SELECT *
	FROM counter;

SELECT * FROM v_nat;
````

````sh
cat counter.sql | sqlite3

# out
0
1
2
3
4
````
