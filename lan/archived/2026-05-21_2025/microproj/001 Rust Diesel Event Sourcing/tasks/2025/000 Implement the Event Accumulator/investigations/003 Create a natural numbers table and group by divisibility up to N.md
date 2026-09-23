---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[002 Investigate group by logic for frame and span to include up to span]]'
context_type: investigation
status: done
---

\#problem #sql

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [002 Investigate group by logic for frame and span to include up to span](002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md)

Spawned in: [^spawn-invst-3a334f](002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md#spawn-invst-3a334f)

# 1 Objective

Given the natural numbers (we can treat sets as tables after all!), group them by divisibility by i in some i in $(2 ... N)$.  $N$ is up to the user.

So the natural numbers table `[0, 1, 2, 3, 4, 5, ...]`  would give us groups like

* `[0, 2, 4, 6, ...]`
* `[0, 3, 6, ...]`
* `[0, 4, 8, ...]`

And so on. Notice that the rows (individual elements) duplicate in the groups, since we're grouping by properties, and not by distinct values.

# 2 Related

* [001 Creating a basic counter with a recursive CTE in sqlite3](../howtos/001%20Creating%20a%20basic%20counter%20with%20a%20recursive%20CTE%20in%20sqlite3.md)
* [002 Creating a basic table duplicator with recursive CTE in sqlite3](../howtos/002%20Creating%20a%20basic%20table%20duplicator%20with%20recursive%20CTE%20in%20sqlite3.md)

# 3 Journal

2025-09-26 Wk 39 Fri - 07:41 +03:00

````sh
mkdir -p ~/tmp/del/*
rm -rf ~/tmp/del/*
````

2025-09-26 Wk 39 Fri - 07:47 +03:00

Spawn [000 Create sqlite3 dbs from sql script](../howtos/000%20Create%20sqlite3%20dbs%20from%20sql%20script.md) ^spawn-howto-c2d6b4

2025-09-26 Wk 39 Fri - 08:11 +03:00

````sql
-- in ~/tmp/del/main.sql
CREATE TABLE nat (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  value INTEGER NOT NULL UNIQUE
);

INSERT INTO nat (value) VALUES 
  (0),
  (1),
  (2),
  (3),
  (4),
  (5),
  (6),
  (7),
  (8),
  (9)
;

.save "main.db"
````

````sh
# in /home/lan/tmp/del
cat main.sql | sqlite3
````

This creates the problem table. Now to create the views.

2025-09-26 Wk 39 Fri - 08:23 +03:00

We're able to create a view with only the even values with

````sql
CREATE VIEW v_nat_even
AS
  SELECT * 
  FROM nat
  WHERE
    nat.value % 2 == 0
;
````

````sh
# in /home/lan/tmp/del
cat main.sql | sqlite3 && vd main.sql

# in visidata
# Enter v_nat_even
````

![Pasted image 20250926082621.png](../../../../../../../../../attachments/Pasted%20image%2020250926082621.png)

2025-09-26 Wk 39 Fri - 23:47 +03:00

So one way we can achieve this is by using an $N$-duplicator, and filtering each duplication of `nat` by divisibility by `dep`.

2025-09-27 Wk 39 Sat - 00:38 +03:00

````sql
-- in divisibility.sql
CREATE TABLE constants (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, 
  max_nat INTEGER NOT NULL,
  max_class INTEGER NOT NULL
);

INSERT INTO constants (max_nat, max_class) VALUES 
	(20, 5);


CREATE VIEW v_nat
AS
	WITH RECURSIVE counter(n) AS (
		SELECT 0
		UNION
		SELECT n + 1 
			FROM counter
      LIMIT (SELECT max_nat FROM constants)
	)
	SELECT *
	FROM counter;


WITH RECURSIVE duplicator(dup, n) AS (
  SELECT 2, n
    FROM v_nat
  UNION
  SELECT dup + 1, n
    FROM duplicator
	  WHERE 
      (dup + 1) < (SELECT max_class FROM constants)
)
SELECT * FROM duplicator
WHERE 
  n % dup == 0
;
````

````sh
cat divisibility.sql | sqlite3 | xargs

# out
2|0 2|2 2|4 2|6 2|8 2|10 2|12 2|14 2|16 2|18 3|0 3|3 3|6 3|9 3|12 3|15 3|18 4|0 4|4 4|8 4|12 4|16
````
