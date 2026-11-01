---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 To materialize grouped events and accumulated objects into tables via software]]'
context_type: investigation
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 To materialize grouped events and accumulated objects into tables via software](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md)

Spawned in: [^spawn-invst-f54b9e](../judgments/000%20To%20materialize%20grouped%20events%20and%20accumulated%20objects%20into%20tables%20via%20software.md#spawn-invst-f54b9e)

# 1 Journal

2025-10-03 Wk 40 Fri - 08:51 +03:00

This [stackoverflow answer](https://stackoverflow.com/a/55139017/6944447) shows that for postgres, has a method for automatically updating a materialized view, but we're in sqlite.

2025-10-03 Wk 40 Fri - 08:55 +03:00

They explain in this [stackoverflow post](https://stackoverflow.com/questions/1374363/how-can-a-materialized-view-be-created-in-sqlite) that there is built-in way in sqlite, and this [comment](https://stackoverflow.com/questions/1374363/how-can-a-materialized-view-be-created-in-sqlite) gives an idea of how this could be implemented in a performant and safe manner.

2025-10-03 Wk 40 Fri - 09:05 +03:00

From to [001 Use of views and CTEs with sqlite3 and diesel-rs](001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md),

[sqldocs.org sqlite views post](https://sqldocs.org/sqlite-database/sqlite-views/)

sqlite documentation can be found here: [sqlite.org docs](https://sqlite.org/docs.html)

2025-10-03 Wk 40 Fri - 09:45 +03:00

[sqlite.org CREATE TRIGGER](https://sqlite.org/lang_createtrigger.html) might be possible to use to update the table when an event is added using the view.

2025-10-03 Wk 40 Fri - 09:49 +03:00

Let's continue the work from the sql in [002 Investigate group by logic for frame and span to include up to span](002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md)

2025-10-03 Wk 40 Fri - 10:16 +03:00

Tutorials on sqlite trigger include [1](https://www.mssqltips.com/sqlservertip/7429/sql-triggers-for-inserts-updates-and-deletes-on-a-table/) [2](https://www.sqlitetutorial.net/sqlite-trigger/) [3 (for instead of)](https://www.sqlitetutorial.net/sqlite-instead-of-triggers/)

Sqlite Trigger Tutorial [2](https://www.sqlitetutorial.net/sqlite-trigger/) includes example usage of `RAISE` for email validation. Adding to [000 Resources encountered during event accumulator impl](../entries/000%20Resources%20encountered%20during%20event%20accumulator%20impl.md)

2025-10-03 Wk 40 Fri - 11:22 +03:00

We need to recompute the content of the history on each new event inserted... This may not be the most efficient, but it would be similar to executing the view query on each request to get the history.

2025-10-03 Wk 40 Fri - 11:29 +03:00

Sqlite Trigger Tutorial [2](https://www.sqlitetutorial.net/sqlite-trigger/) mentions that `INSTEAD OF` can only be used for a trigger on a view. [sqlite.org CREATE TRIGGER](https://sqlite.org/lang_createtrigger.html) confirms this, but it's not what we intend. It only makes a view virtually insertable, but we don't want this.

2025-10-03 Wk 40 Fri - 11:51 +03:00

We can use [sql insert into select tut](https://www.w3schools.com/SQL/sql_insert_into_select.asp) to insert multiple.

2025-10-03 Wk 40 Fri - 11:59 +03:00

````sql
CREATE TRIGGER trg_update_coin_store_hist
	AFTER INSERT ON coin_store_events
BEGIN
	INSERT INTO coin_store_hist
	SELECT t1.*
	FROM v_coin_store_hist AS t1;
END;
````

gives us

````
Parse error near line 131: table coin_store_hist has 8 columns but 7 values were supplied
````

Despite we set it to autoincrement IDs in

````sql
CREATE TABLE coin_store_hist (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  grp_id INTEGER NOT NULL,
  grp_span INTEGER NOT NULL,
  grp_frame INTEGER NOT NULL,
  obj_id INTEGER NOT NULL,
  obj_state TEXT CHECK(obj_state IN ('insert', 'update', 'delete')) NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL
);
````

We can use

````
row_number() over () as id
````

````sql
CREATE TRIGGER trg_update_coin_store_hist
	AFTER INSERT ON coin_store_events
BEGIN
	INSERT INTO coin_store_hist
	SELECT 
    row_number() over () as id,
		t1.*
	FROM v_coin_store_hist AS t1;
END;
````

Now the issue is that it will be duplicating over the items on each insert event. We need to clear it.

As per [sql_delete tut](https://www.w3schools.com/sql/sql_delete.asp),

````sql
CREATE TRIGGER trg_update_coin_store_hist
	AFTER INSERT ON coin_store_events
BEGIN
	DELETE FROM coin_store_hist;
	INSERT INTO coin_store_hist
	SELECT 
    row_number() over () as id,
		t1.*
	FROM v_coin_store_hist AS t1;
END;
````

With this we compute the table each time on event insert!

the events grouped are updated automatically in a similar fashion.

2025-10-03 Wk 40 Fri - 12:13 +03:00

Note that to test this we had to move all the inserts to the end.

Here is the new updated experiment:

````sql
-- in events.sql
CREATE TABLE coin_store_diffs (
  id INTEGER NOT NULL PRIMARY KEY,
  obj_id INTEGER NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL
);

CREATE TABLE coin_store_events (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  opt_diff_id INTEGER NULL REFERENCES coin_store_diffs(id),
  ev_action TEXT CHECK(ev_action IN ('insert', 'update', 'delete', 'open', 'close', 'reopen')) NOT NULL,
  span INTEGER NOT NULL,
  frame INTEGER NOT NULL,
  created_on_ts REAL NOT NULL,
  ev_desc TEXT NOT NULL
);

CREATE TABLE coin_store_events_grouped (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  grp_id INTEGER NOT NULL,
  grp_span INTEGER NOT NULL,
  grp_frame INTEGER NOT NULL,
  grp_created_on_ts REAL NOT NULL,
  dup INTEGER NOT NULL,
  ev_id INTEGER NOT NULL,
  obj_id INTEGER NOT NULL,
  ev_action TEXT CHECK(ev_action IN ('insert', 'update', 'delete', 'open', 'close', 'reopen')) NOT NULL,
  span INTEGER NOT NULL,
  frame INTEGER NOT NULL,
  created_on_ts REAL NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL,
  ev_desc TEXT NOT NULL
);

CREATE TABLE coin_store_hist (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  grp_id INTEGER NOT NULL,
  grp_span INTEGER NOT NULL,
  grp_frame INTEGER NOT NULL,
  obj_id INTEGER NOT NULL,
  obj_state TEXT CHECK(obj_state IN ('insert', 'update', 'delete')) NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL
);

CREATE VIEW v_coin_store_events_grouped AS
  WITH RECURSIVE duplicator(dup, ev_id, obj_id, ev_action, span, frame, created_on_ts, person, coins, ev_desc) AS (
    SELECT 1, t1.id, t2.obj_id, t1.ev_action, t1.span, t1.frame, t1.created_on_ts, t2.person, t2.coins, t1.ev_desc
		FROM coin_store_events AS t1
		INNER JOIN coin_store_diffs AS t2
			ON t1.opt_diff_id = t2.id
		WHERE ev_action != 'open' AND ev_action != 'close' AND ev_action != 'reopen'
    UNION
    SELECT dup + 1, ev_id, obj_id, ev_action, span, frame, created_on_ts, person, coins, ev_desc
		FROM duplicator
		WHERE 
			(dup + 1) <= (
				SELECT COUNT(*) 
				FROM coin_store_events
				WHERE ev_action = 'open'
			)
  )
  SELECT t2.*, t1.*
	FROM duplicator AS t1
	JOIN 
		(
			SELECT row_number() over () as grp_id, * 
			FROM (
				SELECT u1.span AS grp_span, u1.frame AS grp_frame, u1.created_on_ts AS grp_created_on_ts
				FROM coin_store_events AS u1
				WHERE ev_action = 'open'
				)
		) AS t2
	ON t1.dup = t2.grp_id
	WHERE
		(t1.frame == t2.grp_frame AND t1.span == t2.grp_span) OR
		(t1.span < t2.grp_span AND t1.created_on_ts < t2.grp_created_on_ts)
	ORDER BY
		t1.created_on_ts
  ;

CREATE VIEW v_coin_store_hist AS
  WITH 
    aggr AS (
      SELECT
        grp_id, grp_span, grp_frame, obj_id,
        SUM(coins) AS coins
			FROM v_coin_store_events_grouped
			GROUP BY obj_id, grp_id
    ),
    latest AS (
      SELECT 
				grp_id, obj_id, obj_state, person
			FROM (
				SELECT
					grp_id, obj_id, person,
					ev_action AS obj_state,
					ROW_NUMBER() OVER (PARTITION BY grp_id, obj_id ORDER BY created_on_ts DESC) AS rn
				FROM v_coin_store_events_grouped AS t1
			)
			WHERE
				rn = 1
    )
  SELECT 
		a.grp_id, a.grp_span, a.grp_frame, 
	  a.obj_id, l.obj_state, l.person, a.coins
	FROM aggr AS a
	JOIN latest AS l
		ON l.grp_id = a.grp_id AND l.obj_id = a.obj_id
	ORDER BY a.grp_id;

CREATE TRIGGER trg_update_coin_store_events_grouped
	AFTER INSERT ON coin_store_events
BEGIN
	DELETE FROM coin_store_events_grouped;
	INSERT INTO coin_store_events_grouped
	SELECT 
    row_number() over () as id,
		t1.*
	FROM v_coin_store_events_grouped AS t1;
END;


CREATE TRIGGER trg_update_coin_store_hist
	AFTER INSERT ON coin_store_events
BEGIN
	DELETE FROM coin_store_hist;
	INSERT INTO coin_store_hist
	SELECT 
    row_number() over () as id,
		t1.*
	FROM v_coin_store_hist AS t1;
END;

INSERT INTO coin_store_diffs (id, obj_id, person, coins) VALUES 
  (1, 1, 'person001', 10),
  (2, 1, 'Z', 15),
  (3, 1, 'A', 5),
  (4, 2, 'person002', 7),
  (5, 2, 'person002', 14),
  (6, 3, 'person003', 8),
  (7, 1, 'C', 0)
;

INSERT INTO coin_store_events (opt_diff_id, ev_action, span, frame, created_on_ts, ev_desc) VALUES
  (null, 'open', 1, 1, 1758935304.168087602, 'New Years Eve'),
  (1, 'insert', 1, 1, 1758935465.865269566, ''),
  (2, 'update', 1, 1, 1758935520.052601247, ''),
  (3, 'update', 1, 1, 1758935543.867800435, ''),
  (null, 'open', 2, 1, unixepoch('subsec') + 0.1, 'New Years Eve Plan 1'),
  (4, 'insert', 2, 1, unixepoch('subsec') + 0.2, ''),
  (5, 'update', 2, 1, unixepoch('subsec') + 0.3, ''),
  (null, 'close', 2, 1, unixepoch('subsec') + 0.4, 'New Years Eve Plan 1'),
  (6, 'insert', 1, 1, unixepoch('subsec') + 0.5, ''),
  (7, 'delete', 1, 1, unixepoch('subsec') + 0.6, ''),
  (null, 'open', 2, 2, unixepoch('subsec') + 0.7, 'New Years Eve Plan 2'),
  (null, 'open', 1, 2, unixepoch('subsec') + 0.8, 'Birthday Party')
;

.save "events.db"
````

````sh
cat events.sql | sqlite3 && vd events.db
````

![Pasted image 20251003121458.png](../../../../../../../../../attachments/Pasted%20image%2020251003121458.png)

![Pasted image 20251003121525.png](../../../../../../../../../attachments/Pasted%20image%2020251003121525.png)

The views have been materialized into tables, all in SQL!

2025-10-03 Wk 40 Fri - 12:21 +03:00

Let's test if this also works through visidata.

![Pasted image 20251003122215.png](../../../../../../../../../attachments/Pasted%20image%2020251003122215.png)

````sh
date +%s.%N

# out
1759483426.073928817
````

![Pasted image 20251003122446.png](../../../../../../../../../attachments/Pasted%20image%2020251003122446.png)

![Pasted image 20251003122526.png](../../../../../../../../../attachments/Pasted%20image%2020251003122526.png)

`zCtrl+S` for both to commit the changes.

Nothing shows up right after this, but maybe if we make the changes and restart visidata?

Actually just doing a reload with Ctrl+R brings the changes!

2025-10-03 Wk 40 Fri - 12:37 +03:00

Just gotta be mindful about the event timestamps so that insertions go to the right frames.

![Pasted image 20251003123718.png](../../../../../../../../../attachments/Pasted%20image%2020251003123718.png)

2025-10-03 Wk 40 Fri - 12:38 +03:00

We can do one more thing. In order to give the user general history building capability where they may want to filter the events as they see fit, we can create `coin_store_events_grouped_partial`.  and `v_coin_store_hist_partial`. The idea is that the user can perform an arbitrary filtering on the events in  `coin_store_events_grouped` and store the results in `coin_store_events_grouped_partial` and then `v_coin_store_hist_partial` would accumulate all events from `coin_store_events_grouped_partial` instead. This gives us the ability to see the history as it is cut off at a specific time, or more complex filterations such as undo/redo functionality later on.

2025-10-03 Wk 40 Fri - 13:15 +03:00

2025-10-03 Wk 40 Fri - 13:34 +03:00

It works! An arbitrary query now can be used to update the partial history!

````sql
DELETE FROM coin_store_events_grouped_partial;

INSERT INTO coin_store_events_grouped_partial
SELECT * 
FROM coin_store_events_grouped
WHERE obj_id <> 1;
````

![Pasted image 20251003133514.png](../../../../../../../../../attachments/Pasted%20image%2020251003133514.png)
![Pasted image 20251003133538.png](../../../../../../../../../attachments/Pasted%20image%2020251003133538.png)

2025-10-03 Wk 40 Fri - 13:40 +03:00

Now for the ever growing experiment again. We will simplify all of this with some automation later on.

````sql
-- in events.db
CREATE TABLE coin_store_diffs (
  id INTEGER NOT NULL PRIMARY KEY,
  obj_id INTEGER NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL
);

CREATE TABLE coin_store_events (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  opt_diff_id INTEGER NULL REFERENCES coin_store_diffs(id),
  ev_action TEXT CHECK(ev_action IN ('insert', 'update', 'delete', 'open', 'close', 'reopen')) NOT NULL,
  span INTEGER NOT NULL,
  frame INTEGER NOT NULL,
  created_on_ts REAL NOT NULL,
  ev_desc TEXT NOT NULL
);

CREATE TABLE coin_store_events_grouped (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  grp_id INTEGER NOT NULL,
  grp_span INTEGER NOT NULL,
  grp_frame INTEGER NOT NULL,
  grp_created_on_ts REAL NOT NULL,
  dup INTEGER NOT NULL,
  ev_id INTEGER NOT NULL,
  obj_id INTEGER NOT NULL,
  ev_action TEXT CHECK(ev_action IN ('insert', 'update', 'delete', 'open', 'close', 'reopen')) NOT NULL,
  span INTEGER NOT NULL,
  frame INTEGER NOT NULL,
  created_on_ts REAL NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL,
  ev_desc TEXT NOT NULL
);

CREATE TABLE coin_store_events_grouped_partial (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  grp_id INTEGER NOT NULL,
  grp_span INTEGER NOT NULL,
  grp_frame INTEGER NOT NULL,
  grp_created_on_ts REAL NOT NULL,
  dup INTEGER NOT NULL,
  ev_id INTEGER NOT NULL,
  obj_id INTEGER NOT NULL,
  ev_action TEXT CHECK(ev_action IN ('insert', 'update', 'delete', 'open', 'close', 'reopen')) NOT NULL,
  span INTEGER NOT NULL,
  frame INTEGER NOT NULL,
  created_on_ts REAL NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL,
  ev_desc TEXT NOT NULL
);

CREATE TABLE coin_store_hist (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  grp_id INTEGER NOT NULL,
  grp_span INTEGER NOT NULL,
  grp_frame INTEGER NOT NULL,
  obj_id INTEGER NOT NULL,
  obj_state TEXT CHECK(obj_state IN ('insert', 'update', 'delete')) NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL
);

CREATE TABLE coin_store_hist_partial (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  grp_id INTEGER NOT NULL,
  grp_span INTEGER NOT NULL,
  grp_frame INTEGER NOT NULL,
  obj_id INTEGER NOT NULL,
  obj_state TEXT CHECK(obj_state IN ('insert', 'update', 'delete')) NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL
);

CREATE VIEW v_coin_store_events_grouped AS
  WITH RECURSIVE duplicator(dup, ev_id, obj_id, ev_action, span, frame, created_on_ts, person, coins, ev_desc) AS (
    SELECT 1, t1.id, t2.obj_id, t1.ev_action, t1.span, t1.frame, t1.created_on_ts, t2.person, t2.coins, t1.ev_desc
		FROM coin_store_events AS t1
		INNER JOIN coin_store_diffs AS t2
			ON t1.opt_diff_id = t2.id
		WHERE ev_action != 'open' AND ev_action != 'close' AND ev_action != 'reopen'
    UNION
    SELECT dup + 1, ev_id, obj_id, ev_action, span, frame, created_on_ts, person, coins, ev_desc
		FROM duplicator
		WHERE 
			(dup + 1) <= (
				SELECT COUNT(*) 
				FROM coin_store_events
				WHERE ev_action = 'open'
			)
  )
  SELECT t2.*, t1.*
	FROM duplicator AS t1
	JOIN 
		(
			SELECT row_number() over () as grp_id, * 
			FROM (
				SELECT u1.span AS grp_span, u1.frame AS grp_frame, u1.created_on_ts AS grp_created_on_ts
				FROM coin_store_events AS u1
				WHERE ev_action = 'open'
				)
		) AS t2
	ON t1.dup = t2.grp_id
	WHERE
		(t1.frame == t2.grp_frame AND t1.span == t2.grp_span) OR
		(t1.span < t2.grp_span AND t1.created_on_ts < t2.grp_created_on_ts)
	ORDER BY
		t1.created_on_ts
  ;

CREATE VIEW v_coin_store_hist AS
  WITH 
    aggr AS (
      SELECT
        grp_id, grp_span, grp_frame, obj_id,
        SUM(coins) AS coins
			FROM v_coin_store_events_grouped
			GROUP BY obj_id, grp_id
    ),
    latest AS (
      SELECT 
				grp_id, obj_id, obj_state, person
			FROM (
				SELECT
					grp_id, obj_id, person,
					ev_action AS obj_state,
					ROW_NUMBER() OVER (PARTITION BY grp_id, obj_id ORDER BY created_on_ts DESC) AS rn
				FROM v_coin_store_events_grouped AS t1
			)
			WHERE
				rn = 1
    )
  SELECT 
		a.grp_id, a.grp_span, a.grp_frame, 
	  a.obj_id, l.obj_state, l.person, a.coins
	FROM aggr AS a
	JOIN latest AS l
		ON l.grp_id = a.grp_id AND l.obj_id = a.obj_id
	ORDER BY a.grp_id;

CREATE VIEW v_coin_store_hist_partial AS
  WITH 
    aggr AS (
      SELECT
        grp_id, grp_span, grp_frame, obj_id,
        SUM(coins) AS coins
			FROM coin_store_events_grouped_partial
			GROUP BY obj_id, grp_id
    ),
    latest AS (
      SELECT 
				grp_id, obj_id, obj_state, person
			FROM (
				SELECT
					grp_id, obj_id, person,
					ev_action AS obj_state,
					ROW_NUMBER() OVER (PARTITION BY grp_id, obj_id ORDER BY created_on_ts DESC) AS rn
				FROM coin_store_events_grouped_partial AS t1
			)
			WHERE
				rn = 1
    )
  SELECT 
		a.grp_id, a.grp_span, a.grp_frame, 
	  a.obj_id, l.obj_state, l.person, a.coins
	FROM aggr AS a
	JOIN latest AS l
		ON l.grp_id = a.grp_id AND l.obj_id = a.obj_id
	ORDER BY a.grp_id;

CREATE TRIGGER trg_update_coin_store_events_grouped
	AFTER INSERT ON coin_store_events
BEGIN
	DELETE FROM coin_store_events_grouped;
	INSERT INTO coin_store_events_grouped
	SELECT 
    row_number() over () as id,
		t1.*
	FROM v_coin_store_events_grouped AS t1;
END;


CREATE TRIGGER trg_update_coin_store_hist
	AFTER INSERT ON coin_store_events
BEGIN
	DELETE FROM coin_store_hist;
	INSERT INTO coin_store_hist
	SELECT 
    row_number() over () as id,
		t1.*
	FROM v_coin_store_hist AS t1;
END;

CREATE TRIGGER trg_update_coin_store_hist_partial
	AFTER INSERT ON coin_store_events_grouped_partial
BEGIN
	DELETE FROM coin_store_hist_partial;
	INSERT INTO coin_store_hist_partial
	SELECT 
    row_number() over () as id,
		t1.*
	FROM v_coin_store_hist_partial AS t1;
END;

INSERT INTO coin_store_diffs (id, obj_id, person, coins) VALUES 
  (1, 1, 'person001', 10),
  (2, 1, 'Z', 15),
  (3, 1, 'A', 5),
  (4, 2, 'person002', 7),
  (5, 2, 'person002', 14),
  (6, 3, 'person003', 8),
  (7, 1, 'C', 0)
;

INSERT INTO coin_store_events (opt_diff_id, ev_action, span, frame, created_on_ts, ev_desc) VALUES
  (null, 'open', 1, 1, 1758935304.168087602, 'New Years Eve'),
  (1, 'insert', 1, 1, 1758935465.865269566, ''),
  (2, 'update', 1, 1, 1758935520.052601247, ''),
  (3, 'update', 1, 1, 1758935543.867800435, ''),
  (null, 'open', 2, 1, unixepoch('subsec') + 0.1, 'New Years Eve Plan 1'),
  (4, 'insert', 2, 1, unixepoch('subsec') + 0.2, ''),
  (5, 'update', 2, 1, unixepoch('subsec') + 0.3, ''),
  (null, 'close', 2, 1, unixepoch('subsec') + 0.4, 'New Years Eve Plan 1'),
  (6, 'insert', 1, 1, unixepoch('subsec') + 0.5, ''),
  (7, 'delete', 1, 1, unixepoch('subsec') + 0.6, ''),
  (null, 'open', 2, 2, unixepoch('subsec') + 0.7, 'New Years Eve Plan 2'),
  (null, 'open', 1, 2, unixepoch('subsec') + 0.8, 'Birthday Party')
;

DELETE FROM coin_store_events_grouped_partial;

INSERT INTO coin_store_events_grouped_partial
SELECT * 
FROM coin_store_events_grouped
WHERE obj_id <> 1;

.save "events.db"
````
