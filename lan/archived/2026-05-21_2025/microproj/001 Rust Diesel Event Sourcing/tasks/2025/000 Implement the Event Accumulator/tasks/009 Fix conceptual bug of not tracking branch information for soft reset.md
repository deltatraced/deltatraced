---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 Implement the Event Accumulator]]'
context_type: task
status: todo
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned in: [^spawn-task-0f0f75](../000%20Implement%20the%20Event%20Accumulator.md#spawn-task-0f0f75)

# 1 Journal

2025-11-22 Wk 47 Sat - 16:28 +03:00

Local resets cannot work because suppose that span $n+1$ frame $f_{n+1}$ requires the same events as span $n$ frame $f_n$ . It cannot inherit events from span $n$ frame $f_n - 1$. It also cannot inherit events from frame $f_{n-1} - 1$

In other words no single-integer solution is sufficient to describe the stack inheritance. We need the entire branch history to classify what events reside in a current frame.

This could possibly be handled mostly by software.

One proposal to fix this is to assign a branch string value for each event. This string is computed in software. For the SQL, what is of interest is to check for a given frame's branch to `starts_with` an event's. If it does, it belongs to the same branch history.

[w3schools SQL LIKE](https://www.w3schools.com/SQL/sql_like.asp) offers the `LIKE` operator which can be used for this.

2025-11-22 Wk 47 Sat - 16:51 +03:00

The implementation of the automation is in `/home/lan/src/cloned/gh/dbmint/dbmts_rs/src/event_sourcing.rs`

We can experiment over with the generated `up.sql` from `up.dbmts` in `/home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store`

Let's add a text property of `branch` to `coin_store_events` and similar.

The `branch` field should be a string list of frames $f_n$ for every span $n$  followed by commas. like `0,1,3,`, etc.

Just like we have span $\to$ grp_span and frame $\to$ grp_frame in `coin_store_hist` and similar, we also add grp_branch.

Proceed to also add branch for `v_coin_store_events_grouped` and similar similar to how we add span and frame.

Then this conditional:

````SQL
WHERE
(t1.frame == t2.grp_frame AND t1.span == t2.grp_span) OR
(t1.span < t2.grp_span AND t1.created_on_ts < t2.grp_created_on_ts)
````

States that events in the given frame and span are of course included, or those of prior span and prior creation time.

This second condition is where the bug is. An unwanted frame $f_n - 1$ satisfies the condition span $n < n + 1$ and being created prior.  Where it would fail is that $f_n - 1$ should not be found in the branch string of values.

````sql
WHERE
  (t1.frame == t2.grp_frame AND t1.span == t2.grp_span) OR
  (t1.span < t2.grp_span AND t1.created_on_ts < t2.grp_created_on_ts AND t2.grp_branch LIKE t1.branch)
````

With this, it should be that we're referring to $f_n$ and not some $f_n + m$ not of interest, for any span $n$.

We have a choice of also ensuring in the first condition

````sql
  (t1.frame == t2.grp_frame AND t1.span == t2.grp_span) OR
````

that they have the exact branch. This is to be expected, so let us add it for more structural integrity.

````
WHERE
  (t1.frame == t2.grp_frame AND t1.span == t2.grp_span AND t1.branch == t2.grp_branch) OR
  (t1.span < t2.grp_span AND t1.created_on_ts < t2.grp_created_on_ts AND t2.grp_branch LIKE t1.branch)
````

After all, events created in the same span and frame, are in the identical branch, not even just its earlier string of $f_n$ .

Of course the cost of this is that software now needs to ensure to generate valid branch strings that satisfy this property, but since this should all be abstracted automation, it should be fine.

2025-11-22 Wk 47 Sat - 17:17 +03:00

Now let's do some testing to ensure our patch works. If so, then we update the automation that generated it in the `*.dbmts` file format.

Similar to testing data in [002 Investigate group by logic for frame and span to include up to span](../investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md),

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store
cp up.sql up2.sql
cat <(cat << 'EOF'
INSERT INTO coin_store_diffs (id, obj_id, person, coins) VALUES 
	(1, 1, 'P1', 11),
	(2, 2, 'P2', 12),
	(3, 1, 'P1', 100),
	(4, 1, 'P1', 200),
	(5, 2, 'P2', 300)
;

INSERT INTO coin_store_events (opt_diff_id, ev_action, span, frame, branch, created_on_ts, ev_desc) VALUES
	(null, 'open', 1, 1, '1,', unixepoch('subsec')+0, 'open'),
	(1, 'insert', 1, 1, '1,', unixepoch('subsec')+1, ''),
	(2, 'insert', 1, 1, '1,', unixepoch('subsec')+2, ''),
	
	(null, 'open', 1, 2, '2,', unixepoch('subsec')+3, 'open'),
	(3, 'insert', 1, 2, '2,', unixepoch('subsec')+4, ''),
	
	(null, 'open', 2, 1, '1,1,', unixepoch('subsec')+5, 'open'),
	(4, 'update', 2, 1, '1,1,', unixepoch('subsec')+6, ''),
	(5, 'update', 2, 1, '1,1,', unixepoch('subsec')+7, ''),
	
	(null, 'close', 1, 1, '1,', unixepoch('subsec')+8, 'close')
;

.save "up.db"
EOF
) >> up2.sql
cat up2.sql | sqlite3
````

````
Parse error near line 212: table duplicator has 11 values for 10 columns
````

Just add `branch` to this line:

````
WITH RECURSIVE duplicator(dup, ev_id, obj_id, ev_action, span, frame, branch, created_on_ts, person, coins, ev_desc) AS (
````

And also add `grp_branch` to `coin_store_events_grouped` and `coin_store_events_grouped_partial`

````
Parse error near line 213: table coin_store_events_grouped has 16 columns but 17 values were supplied
````

Okay issues resolved.

2025-11-22 Wk 47 Sat - 18:23 +03:00

via `vd ,` for selecting a row and `vd Edit>Copy>to system clipboard>selected rows` and specifying `csv` and putting the results in a file `a` to be fed to `cat a | csvtomd`, we get the following for `v_coin_store_hist`:

(You can also do `vd gt gShift+Y csv` then put content in file `a` and `cat a | csvtomd`. Putting this in howto)

Spawn [003 Copy visidata table to obsidian](../howtos/003%20Copy%20visidata%20table%20to%20obsidian.md) ^spawn-howto-499709

|id|grp_id|grp_span|grp_frame|grp_branch|obj_id|obj_state|person|coins|
|--|------|--------|---------|----------|------|---------|------|-----|
|1|1|1|1|1,|1|insert|P1|11|
|2|1|1|1|1,|2|insert|P2|12|
|3|2|1|2|2,|1|insert|P1|100|
|4|3|2|1|1,1,|1|update|P1|200|
|5|3|2|1|1,1,|2|update|P2|300|

It does not seem that `LIKE` is behaving as I expect for substring.

2025-11-22 Wk 47 Sat - 18:37 +03:00

````
WHERE
  (t1.frame == t2.grp_frame AND t1.span == t2.grp_span AND t1.branch == t2.grp_branch) OR
  (t1.span < t2.grp_span AND t1.created_on_ts < t2.grp_created_on_ts AND t2.grp_branch LIKE t1.branch || '%')
````

specifically `t2.grp_branch LIKE t1.branch || '%'` with the addition of `%` to form a correct pattern for `LIKE`.

This [microsoft doc](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/string-concatenation-pipes-transact-sql?view=sql-server-ver17) explains `||` is string concat.

In [w3schools sql wildcards](https://www.w3schools.com/SQL/sql_wildcards.asp) the example also explains that starts with is implemented by ending the string with `%`.

2025-11-22 Wk 47 Sat - 18:44 +03:00

Now `v_coin_store_hist` shows:

|grp_id|grp_span|grp_frame|grp_branch|obj_id|obj_state|person|coins|
|------|--------|---------|----------|------|---------|------|-----|
|1|1|1|1,|1|insert|P1|11|
|1|1|1|1,|2|insert|P2|12|
|2|1|2|2,|1|insert|P1|100|
|3|2|1|1,1,|1|update|P1|211|
|3|2|1|1,1,|2|update|P2|312|

It seems like it works!

Another change to make is that `grp_id`, `grp_span`, and `grp_frame` are all duplicate information now and are all unified as `grp_branch`. Recall that `grp_id` was created because there was no unified unique value for each "world" or frame. But `grp_branch` is perfectly unique and describes a world of affairs. `grp_span` is simply the count of elements in `grp_branch`, and `grp_frame` is just the last entry. They do not need to exist separately.

`grp_span` and `grp_frame` were needed by the conditional

````sql
WHERE
  (t1.frame == t2.grp_frame AND t1.span == t2.grp_span AND t1.branch == t2.grp_branch) OR
  (t1.span < t2.grp_span AND t1.created_on_ts < t2.grp_created_on_ts AND t2.grp_branch LIKE t1.branch || '%')
````

But notice that `t1.span < t2.grp_span` would be covered by `t2.grp_branch LIKE t1.branch || '%'` and similarly `t1.frame == t2.grp_frame AND t1.span == t2.grp_span` are covered by `t1.branch == t2.grp_branch`.

Let's remove `grp_id` and all instances of span and frame entirely, and rename `branch` to `ev_branch`.

2025-11-22 Wk 47 Sat - 19:02 +03:00

There was a place in creating `v_coin_store_events_grouped` that we used `grp_id`:

````
ON t1.dup = t2.grp_id
````

I guess it still had this function of being a numerical nth branch that you won't find from in the content of `grp_branch`, so let's keep `grp_id`

This simplifies our `v_coin_store_events_grouped` filter logic to

````sql
WHERE
  t1.branch == t2.grp_branch OR
  (t1.created_on_ts < t2.grp_created_on_ts AND t2.grp_branch LIKE t1.branch || '%')
````

This also could be wrong. Thanks to `grp_span` and `grp_frame` we had a coordinate for specifying a frame of interest at a specific depth and value. But software can handle this, instead of grp_span, it's just the depth. So we can refer to `1,1,1,3` in short hand, depth 4 frame 3 and it would uniquely identify it. But we don't need this information duplicated into separate `grp_span` and `grp_frame`. This can be checked directly against `grp_branch`. More importantly, It is also a flawed coordinate state. Because `1,1,2,3` can also exist at depth 4 frame 3, and it no longer uniquely identifies the frame of interest. This bug was caused by me trying to fit a 2D cartesian cooridnate system to the space of all possible branching event histories, and that space is simply not large enough. Because this space is not of size $(N, N)$. It's of size $N^D$ where $D$ is the the maximum depth.

The user has to specify a world by its forking history.

2025-11-22 Wk 47 Sat - 19:15 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store
cp up.sql up2.sql
cat <(cat << 'EOF'
INSERT INTO coin_store_diffs (id, obj_id, person, coins) VALUES 
	(1, 1, 'P1', 11),
	(2, 2, 'P2', 12),
	(3, 1, 'P1', 100),
	(4, 1, 'P1', 200),
	(5, 2, 'P2', 300),
	(6, 1, 'P1', 0)
;

INSERT INTO coin_store_events (opt_diff_id, ev_action, ev_branch, created_on_ts, ev_desc) VALUES
	(null, 'open', '1,', unixepoch('subsec')+0, 'open'),
	(1, 'insert', '1,', unixepoch('subsec')+1, ''),
	(2, 'insert', '1,', unixepoch('subsec')+2, ''),
	
	(null, 'open', '2,', unixepoch('subsec')+3, 'open'),
	(3, 'insert', '2,', unixepoch('subsec')+4, ''),
	
	(null, 'open', '1,1,', unixepoch('subsec')+5, 'open'),
	(4, 'update', '1,1,', unixepoch('subsec')+6, ''),
	(5, 'update', '1,1,', unixepoch('subsec')+7, ''),
	
	(null, 'open', '2,1,', unixepoch('subsec')+8, 'open'),
	
	(null, 'open', '2,1,1,', unixepoch('subsec')+9, 'open'),
	(6, 'delete', '2,1,1,', unixepoch('subsec')+10, ''),
	
	(null, 'close', '1,', unixepoch('subsec')+11, 'close')
;

.save "up.db"
EOF
) >> up2.sql
cat up2.sql | sqlite3
````

Corresponding `coin_store_events_grouped`:

|id|grp_id|grp_branch|grp_created_on_ts|dup|ev_id|obj_id|ev_action|ev_branch|created_on_ts|person|coins|ev_desc|
|--|------|----------|-----------------|---|-----|------|---------|---------|-------------|------|-----|-------|
|1|1|1,|1763831513.18|1|2|1|insert|1,|1763831514.18|P1|11||
|3|1|1,|1763831513.18|1|3|2|insert|1,|1763831515.18|P2|12||
|2|3|1,1,|1763831518.18|3|2|1|insert|1,|1763831514.18|P1|11||
|4|3|1,1,|1763831518.18|3|3|2|insert|1,|1763831515.18|P2|12||
|8|3|1,1,|1763831518.18|3|7|1|update|1,1,|1763831519.18|P1|200||
|9|3|1,1,|1763831518.18|3|8|2|update|1,1,|1763831520.18|P2|300||
|5|2|2,|1763831516.18|2|5|1|insert|2,|1763831517.18|P1|100||
|6|4|2,1,|1763831521.18|4|5|1|insert|2,|1763831517.18|P1|100||
|7|5|2,1,1,|1763831522.18|5|5|1|insert|2,|1763831517.18|P1|100||
|10|5|2,1,1,|1763831522.18|5|11|1|delete|2,1,1,|1763831523.18|P1|0||

Corresponding `coin_store_hist`:

|id|grp_id|grp_branch|obj_id|obj_state|person|coins|
|--|------|----------|------|---------|------|-----|
|1|1|1,|1|insert|P1|11|
|2|1|1,|2|insert|P2|12|
|3|2|2,|1|insert|P1|100|
|4|3|1,1,|1|update|P1|211|
|5|3|1,1,|2|update|P2|312|
|6|4|2,1,|1|insert|P1|100|
|7|5|2,1,1,|1|delete|P1|100|

2025-11-22 Wk 47 Sat - 20:28 +03:00

Okay this looks good. What's left is to update the automation in `*.dbmts` via `sql_derive_event_sourcing` in `/home/lan/src/cloned/gh/dbmint/dbmts_rs/src/event_sourcing.rs` accordingly.

The demo code automation also needs to update to accommodate the change here.

2025-11-22 Wk 47 Sat - 21:06 +03:00

````sh
# in /home/lan/src/cloned/gh/dbmint/dbmts_rs
git commit

# out
Trim Trailing Whitespace.................................................Passed
Check Yaml...........................................(no files to check)Skipped
Check for added large files..............................................Passed
Check formatting.........................................................Passed
Run tests................................................................Passed
Check clippy lints.......................................................Passed
[main be9c5fe] Fix soft reset bug by using branch encoding in es
 1 file changed, 18 insertions(+), 25 deletions(-)
````
