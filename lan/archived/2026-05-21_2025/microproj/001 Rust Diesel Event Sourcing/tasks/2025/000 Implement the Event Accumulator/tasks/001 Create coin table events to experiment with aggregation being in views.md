---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 Implement the Event Accumulator]]'
context_type: task
status: done
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned in: [^spawn-task-9b8a2b](../000%20Implement%20the%20Event%20Accumulator.md#spawn-task-9b8a2b)

# 1 Related

[001 Use of views and CTEs with sqlite3 and diesel-rs](../investigations/001%20Use%20of%20views%20and%20CTEs%20with%20sqlite3%20and%20diesel-rs.md)

# 2 Journal

2025-09-21 Wk 38 Sun - 23:21 +03:00

Currently our schema for credit is

````mermaid
erDiagram
	credit_store {
		key_t id
		Unique[text] person
		i32 credits
	}
	
	credit_store_head {
		key_t id
		Unique[text] person
		i32 credits
	}
	
	credit_store_version {
		key_t id
		Option[key_t] opt_event_id
	}
	credit_store_version ||--|| credit_store_events : compose
	
	credit_store_events {
		key_t id
		string person
		i32 credits
		Optional[u32] opt_object_id
		Optional[key_t] opt_event_id
		Optional[u32] opt_event_arg
		u32 event_stack_level
		EventAction event_action
		timestamp_t created_on
	}
	credit_store_events ||--o{ credit_store_events : compose
````

And our actions are:

|Event Action|Description|
|------------|-----------|
|insert `ID`|Insert a new object with a new `object_id`.|
|update `ID`|Set the new values for an existing `object_id`|
|delete `ID`|Delete an object given by `object_id`|
|pop|Objects state returns to previous `event_lifetime`|
|reset|Delete all objects.|
|init|Set the initial state from a base table|
|undo `N` \[`ID`\]|undo the last `N` changes. Optionally, an `object_id` can be given to undo only its last `N` changes.|
|redo `N` \[`ID`\]|Cancels an `undo` or acts as no-op otherwise.|
|seek `EID`|Seeks an event id, overwriting all object states with that version|

2025-09-22 Wk 39 Mon - 00:13 +03:00

All this needs to be simplified. No seeks or undos or redos or pops. And events can be local if they include an object, or global if they do not.

````mermaid
erDiagram
	coin_store_objects {
		key_t id
		string person
		i32 coins
	}

	coin_store_events {
		key_t id
		Option[key_t] opt_object_id
		EventAction ev_action
		u32 ev_scale
		u32 ev_frame
		text ev_tags
		timestamp_t created_on
	}
	coin_store_events ||--|| coin_store_objects : compose
````

With actions

|Event Action|Description|
|------------|-----------|
|insert `OID`|Insert a new object with a new `object_id`.|
|update `OID`|Set the new values for an existing `object_id`|
|delete `OID`|Delete an object given by `object_id`|
|frame `s` `f`|Frame events create a new frame with scale `s` and frame no `f`|

2025-09-26 Wk 39 Fri - 01:38 +03:00

Note that a global frame event is only necessary for scale-relative empty state of affairs.  For example if we were in scale 0 frame 0, then `frame 0 1` would produce a full reset, since there's nothing yet on frame 1, and scale 0 is the lowest and therefore no objects from prior scales persist.

2025-09-26 Wk 39 Fri

2025-09-26 Wk 39 Fri01:50 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
diesel migration generate create_coin_store

# out
Creating migrations/2025-09-25-225000_create_coin_store/up.sql
Creating migrations/2025-09-25-225000_create_coin_store/down.sql
````

2025-09-26 Wk 39 Fri - 02:16 +03:00

We need to add the summing-aggregating object state described in [000 Implement the Event Accumulator > ^recall-244d1b](../000%20Implement%20the%20Event%20Accumulator.md#recall-244d1b)

But this also means that since there's a 1-to-1 correspondence between events and "objects", those aren't full objects but just diffs. Let's rename them to `x_diffs` and have the object id be maintained separately by the event.

````mermaid
erDiagram
	coin_store_diffs {
		key_t id
		key_t obj_id
		string person
		i32 coins
	}

	coin_store_events {
		key_t id
		Option[key_t] opt_diff_id
		i32 obj_state
		EventAction ev_action
		u32 ev_scale
		u32 ev_frame
		text ev_tags
		timestamp_t created_on
	}
	coin_store_events ||--|| coin_store_diffs : compose
````

Both the events and the diffs are append-only stores. They are separated for conceptual cohesion. The diff portion is what would change between one different events. To view the full event data, just join with its corresponding diff, if any. Another reason is so that we can have the global/local event separation signified by a missing diff, since `frame s f` touches no particular object.

`opt_obj_id` is a key that references no table, it's just for accumulation of the events. If we go the managed head route, it would be a unique id there. If we go with views, it would be unique there.

Both `opt_obj_id` and `opt_diff_id` may be null together, or present together, but you cannot null one and not the other, this should result in an error. This would be worse with `obj_state`, now all three may be present or may be null. T

To simplify, we moved `obj_id` and `obj_state` to the diff table. Now they must always be present if there's a diff.

Differential values in diffs and how diffs accumulate should be determined by software.

2025-09-26 Wk 39 Fri - 02:36 +03:00

Technically now, `ev_action` has been made redundant by `obj_state` and presence of `opt_diff_id`. If no diff is present, we know it's a `frame`. If a diff is present, we can check the `obj_state` and know that $1$ is insert, $\gt 1$ is update, and $0$ is delete.

(update)
But actually this is only in the case that the diffs accumulate themselves, and they shouldn't. So `obj_state` will show $1$ on both insert and update and $-N$ on delete, making a delete aggregating. So when we delete objects, we need to know the count of the preceding diffs, but on insert and update, we always say $1$, so we can still keep the event action to be a source of truth for the kind of event this is.

2025-09-26 Wk 39 Fri - 02:57 +03:00

Update $N$ to $-N$. Deletions need to be negative to balance the previous insert + updates on sum aggregates.

We can move `obj_state` back to events. Global events will just need to know to not affect it in aggregates by setting it to `0`. This way it's also not the responsibility of a new event implementer to define how object states aggregate.

(/update)

2025-09-26 Wk 39 Fri - 02:45 +03:00

````sql
-- in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store/up.sql
-- Your SQL goes here
CREATE TABLE coin_store_diffs (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  obj_id INTEGER NOT NULL,
  person TEXT NOT NULL,
  coins INTEGER NOT NULL
);

CREATE TABLE coin_store_events (
  id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
  opt_diff_id INTEGER NULL REFERENCES coin_store_diffs(id),
  transactions INTEGER NOT NULL,
  ev_action TEXT CHECK(ev_action IN ('insert', 'update', 'delete', 'frame')) NOT NULL,
  ev_scale INTEGER NOT NULL,
  ev_frame INTEGER NOT NULL,
  ev_tags TEXT NOT NULL,
  created_on TEXT NOT NULL,
);
````

````sql
-- in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/migrations/2025-09-25-225000_create_coin_store/down.sql
-- This file should undo anything in `up.sql`
DROP TABLE coin_store_diffs;
DROP TABLE coin_store_events;
````

Now let's do a full cycle regeneration according to the README,

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
source ./.env && rm $DATABASE_URL; diesel migration run && python3 scripts/diesel-postprocess.py

# out
Running migration 2025-09-12-162639_create_credit_store
Running migration 2025-09-25-225000_create_coin_store
````

2025-09-26 Wk 39 Fri - 03:05 +03:00

Had to update `event_action` in the CHECK to `ev_action` for it to compile, but otherwise good!

2025-09-26 Wk 39 Fri - 03:07 +03:00

Let's also update `EventAction` to only have the four states, and change that also in credit store.

2025-09-26 Wk 39 Fri - 03:32 +03:00

Let's update `obj_state` to just `transactions`. It makes more sense then it's a number, and an object that aggregates to 0 transactions is as good as non-existent.

2025-09-26 Wk 39 Fri - 04:09 +03:00

So in our view, we want there to be unique states of affairs per frame, each aggregating only *up to* its scale. So we can't treat the frames as distinct worlds as might be the case if we just grouped by them.

Spawn [002 Investigate group by logic for frame and span to include up to span](../investigations/002%20Investigate%20group%20by%20logic%20for%20frame%20and%20span%20to%20include%20up%20to%20span.md) ^spawn-invst-bd7e4d

2025-09-26 Wk 39 Fri - 04:36 +03:00

Renamed and reordered some items. `ev_tags` are just event specific, but `scale` and `frame` do appear in view aggregates, so removed `ev_` from them.

2025-09-26 Wk 39 Fri - 10:37 +03:00

So far, we have

````mermaid
erDiagram
	coin_store_diffs {
		key_t id
		key_t obj_id
		string person
		i32 coins
	}

	coin_store_events {
		key_t id
		Option[key_t] opt_diff_id
		i32 transactions
		EventAction ev_action
		u32 span
		u32 frame
		text ev_tags
		u32 created_on_ts
	}
	coin_store_events ||--|| coin_store_diffs : compose
	
	v_coin_store {
		key_t id
		string person
		i32 coins
		i32 transactions
		u32 span
		u32 frame
		u32 grp_span
		u32 grp_frame
		u32 grp_last_updated_on_ts
	}
````

2025-09-28 Wk 39 Sun - 00:51 +03:00

Now it is

````mermaid
erDiagram
	coin_store_diffs {
		key_t id
		key_t obj_id
		string person
		i32 coins
	}

	coin_store_events {
		key_t id
		Option[key_t] opt_diff_id
		i32 transactions
		EventAction ev_action
		u32 span
		u32 frame
		f64 created_on_ts
	}
	coin_store_events ||--|| coin_store_diffs : compose
	
	v_coin_store_events_grouped {
		key_t dup
		key_t obj_id
		string person
		i32 coins
		i32 transactions
		u32 span
		u32 frame
		f64 created_on_ts
		key_t grp_id
		u32 grp_span
		u32 grp_frame
		f64 grp_created_on_ts
	}
	
	v_coin_store {
		key_t obj_id
		key_t grp_id
		u32 grp_span
		u32 grp_frame
		i32 transactions
		string person
		i32 coins
	}
````

2025-10-02 Wk 40 Thu - 10:21 +03:00

We removed transactions, since we just get latest, the latest event for an object is its object state.

````mermaid
erDiagram
	coin_store_diffs {
		key_t id
		key_t obj_id
		string person
		i32 coins
	}

	coin_store_events {
		key_t id
		Option[key_t] opt_diff_id
		EventAction ev_action
		u32 span
		u32 frame
		f64 created_on_ts
		ev_desc text
	}
	coin_store_events ||--|| coin_store_diffs : compose
	
	v_coin_store_events_grouped {
		key_t grp_id
		u32 grp_span
		u32 grp_frame
		f64 grp_created_on_ts
		key_t dup
		key_t ev_id
		key_t obj_id
		EventAction ev_action
		u32 span
		u32 frame
		f64 created_on_ts
		string person
		i32 coins
		string ev_desc
	}
	
	v_coin_store {
		key_t grp_id
		u32 grp_span
		u32 grp_frame
		key_t obj_id
		ObjectState obj_state
		string person
		i32 coins
	}
````

The event actions have been updated. We can now close frames, and reopen them. This doesn't interfere heavily with the sourcing logic, which only sources from lower spans on frame creation, but software can use this distinction and it can have a logical relevance. For example we should not be able to append to a closed frame. Similarly, it's up to software whether to allow reopen events.

|Event Action|Description|
|------------|-----------|
|insert `OID`|Insert a new object with a new `object_id`.|
|update `OID`|Set the new values for an existing `object_id`.|
|delete `OID`|Delete an object given by `object_id`.|
|open `s` `f`|Creates a new frame `f` at span `s`.|
|close `s` `f`|Closes a frame `f` at span `s`. Events can longer append there.|
|reopen `s` `f`|Reopens a frame `f` at span `s`. Events can append there again.|

`ObjectState` is just `EventAction` but without open/close/reopen: `insert`, `update,` `delete`.

2025-10-02 Wk 40 Thu - 10:50 +03:00

Now let's look at how this interacts with diesel

Spawn [002 Add event accumulation events through diesel](002%20Add%20event%20accumulation%20events%20through%20diesel.md) ^spawn-task-48bbe6
