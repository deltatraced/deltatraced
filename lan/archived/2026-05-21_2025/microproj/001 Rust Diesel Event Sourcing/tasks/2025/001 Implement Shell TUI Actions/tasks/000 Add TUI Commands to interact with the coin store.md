---
parent: '[[001 Implement Shell TUI Actions]]'
spawned_by: '[[001 Implement Shell TUI Actions]]'
context_type: task
status: todo
---

Parent: [001 Implement Shell TUI Actions](../001%20Implement%20Shell%20TUI%20Actions.md)

Spawned by: [001 Implement Shell TUI Actions](../001%20Implement%20Shell%20TUI%20Actions.md)

Spawned in: [^spawn-task-519857](../001%20Implement%20Shell%20TUI%20Actions.md#spawn-task-519857)

# 1 Journal

2025-10-21 Wk 43 Tue - 20:26 +03:00

Let's just set their coins to 0 to start with.

2025-10-21 Wk 43 Tue - 22:35 +03:00

````rust
let results = coin_store_events
	.filter(ev_action.eq(EventAction::Open))
//   ~~~~~~
	.select(coin_store::Event::as_select())
	.get_results(conn);
;
````

````rust
error[E0034]: multiple applicable items in scope
   --> src/bin/demo.rs:153:10
    |
153 |         .filter(ev_action.eq(EventAction::Open))
    |          ^^^^^^ multiple `filter` found
    |
note: candidate #1 is defined in the trait `Iterator`
   --> /home/lan/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:892:5
    |
892 | /     fn filter<P>(self, predicate: P) -> Filter<Self, P>
893 | |     where
894 | |         Self: Sized,
895 | |         P: FnMut(&Self::Item) -> bool,
    | |______________________________________^
    = note: candidate #2 is defined in an impl of the trait `FilterDsl` for the type `T`
    = note: candidate #3 is defined in an impl of the trait `diesel::QueryDsl` for the type `T`
help: disambiguate the method for candidate #1
    |
152 -     let results = coin_store_events
153 -         .filter(ev_action.eq(EventAction::Open))
152 +     let results = Iterator::filter(coin_store_events, ev_action.eq(EventAction::Open))
    |
help: disambiguate the method for candidate #2
    |
152 -     let results = coin_store_events
153 -         .filter(ev_action.eq(EventAction::Open))
152 +     let results = FilterDsl::filter(coin_store_events, ev_action.eq(EventAction::Open))
    |
help: disambiguate the method for candidate #3
    |
152 -     let results = coin_store_events
153 -         .filter(ev_action.eq(EventAction::Open))
152 +     let results = diesel::QueryDsl::filter(coin_store_events, ev_action.eq(EventAction::Open))
    |
````

It seems I have to disambiguate filter

````rust
    let results = coin_store_events
        .pipe(|tbl| FilterDsl::filter(tbl, ev_action.eq(EventAction::Open)))
        .select(coin_store::Event::as_select())
        .get_results(conn)
    ;
````

This can work...

2025-10-21 Wk 43 Tue - 23:19 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
git commit -m "adding commands to impl for demo"

# out
Trim Trailing Whitespace.................................................Passed
Check Yaml...........................................(no files to check)Skipped
Check for added large files..............................................Passed
Check formatting.........................................................Passed
Run tests................................................................Passed
Check clippy lints.......................................................Passed
[main f85cc7f] adding commands to impl for demo
 4 files changed, 243 insertions(+), 16 deletions(-)
````

2025-10-22 Wk 43 Wed - 11:13 +03:00

Let's create `obj_id` as determinstic hashes based on the bitpattern of the person's name

There's [gh wassasin/deterministic-hash](https://github.com/wassasin/deterministic-hash)

It provides an [implementation](https://github.com/Wassasin/deterministic-hash/blob/d4d58242bdb5d2fae589b23cc688e28904135215/src/lib.rs#L31C9-L31C15) of `DeterministicHasher<T>` for any derive of `Hash` which seems to expose the trait `Hasher<T>` here.

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
cargo add deterministic-hash

# out
    Updating crates.io index
      Adding deterministic-hash v1.0.2 to dependencies
    Updating crates.io index
    Blocking waiting for file lock on package cache
     Locking 1 package to latest Rust 1.89.0 compatible version
      Adding deterministic-hash v1.0.2
````

2025-10-22 Wk 43 Wed - 11:34 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
cargo add crc

# out
    Updating crates.io index
      Adding crc v3.3.0 to dependencies
    Updating crates.io index
    Blocking waiting for file lock on package cache
     Locking 2 packages to latest Rust 1.89.0 compatible versions
      Adding crc v3.3.0
      Adding crc-catalog v2.4.0
````

````rust
error[E0603]: module `crc32` is private
  --> src/bin/demo.rs:50:52
   |
50 |     let mut hasher = DeterministicHasher::new(crc::crc32::Digest::new(crc::crc32::KOOPMAN));
   |                                                    ^^^^^ private module
   |
note: the module `crc32` is defined here
  --> /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crc-3.3.0/src/lib.rs:36:1
   |
36 | mod crc32;
   | ^^^^^^^^^
help: consider importing this struct instead
   |
50 -     let mut hasher = DeterministicHasher::new(crc::crc32::Digest::new(crc::crc32::KOOPMAN));
50 +     let mut hasher = DeterministicHasher::new(crc::Digest::new(crc::crc32::KOOPMAN));
   |
````

Their [usage](https://github.com/Wassasin/deterministic-hash/blob/d4d58242bdb5d2fae589b23cc688e28904135215/src/lib.rs#L24) of `crc::crc32` is private

Following the suggestion of the error leads to more.

2025-10-22 Wk 43 Wed - 11:59 +03:00

2025-10-22 Wk 43 Wed - 13:33 +03:00

[gh mrhooray/crc-rs](https://github.com/mrhooray/crc-rs)

````rust
const X25: crc::Crc<u16> = crc::Crc::<u16>::new(&crc::CRC_16_IBM_SDLC);
````

2025-10-22 Wk 43 Wed - 17:09 +03:00

Can't seem to use `X25` as a `Hasher` directly, but we can with [docs.rs crc32fast](https://docs.rs/crc32fast/latest/crc32fast/index.html)

This works:

````rust
use crc32fast::Hasher;

let obj_id: u32 = {
	let mut hasher = DeterministicHasher::new(Hasher::new());
	person.hash(&mut hasher);

	hasher.as_inner().clone().finalize()
};
````

2025-10-22 Wk 43 Wed - 17:26 +03:00

2025-10-22 Wk 43 Wed - 22:51 +03:00

For printing wallets and transactions let's use [gh zhiburt/tabled](https://github.com/zhiburt/tabled) for some pretty printing

2025-10-22 Wk 43 Wed - 23:46 +03:00

We also checked on displaying millis back to readable time. We can do it with

````rust
pub fn display_timestamp(timestamp: f32) -> String {
    use chrono::{DateTime, TimeZone, Utc};

    let timestamp_millis = timestamp as i64;

    let seconds = timestamp_millis / 1000;
    let nanoseconds = (timestamp_millis % 1000) * 1_000_000;

    let datetime: DateTime<Utc> = Utc
        .timestamp_opt(seconds, nanoseconds as u32)
        .single()
        .unwrap();

    format!("{}", datetime)
}
````

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
git commit

# out
Trim Trailing Whitespace.................................................Passed
Check Yaml...........................................(no files to check)Skipped
Check for added large files..............................................Passed
Check formatting.........................................................Passed
Run tests................................................................Passed
Check clippy lints.......................................................Passed
[main 0b5ed59] added commands to show wallet and records and add expenses and income
 Date: Wed Oct 22 23:46:12 2025 +0300
 6 files changed, 590 insertions(+), 64 deletions(-)
````

Now we can show wallets, or records. When we show records, we also include descriptions and human readable times.

2025-10-23 Wk 43 Thu - 05:37 +03:00

For undo toggles, we need to operate not on the events grouped but on the general events so that it propagates to all groups. So for displaying purposes we need to join them with diffs. To learn about diesel joins and their ways of getting relations see [diesel.rs guides/relations](https://diesel.rs/guides/relations.html).

2025-10-23 Wk 43 Thu - 06:52 +03:00

````
| coins toggle id
╭────┬─────────┬─────────────────────────────┬────────┬─────────────┬─────────────╮
│ id │ toggled │ timestamp                   │ person │ total_coins │ description │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 9  │ false   │ 2025-10-23 03:49:55.328 UTC │ Lan    │ 0           │ create user │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 10 │ true    │ 2025-10-23 03:49:55.328 UTC │ Lan    │ -1000       │ Big debt    │
╰────┴─────────┴─────────────────────────────┴────────┴─────────────┴─────────────╯
enter Select event id to toggle (u32) or type [q]uit: 
````

Nothing should be toggled off in the beginning.

2025-10-23 Wk 43 Thu - 07:16 +03:00

Seems it only keeps the chosen toggle id and disables everything else.

````rust
// in fn coin_store_toggle_by_id
// TODO added to test toggle logic
set_coin_store_events_partial_to_full(&mut mut_state.conn)
	.map_err(|e| ShiError::General { msg: e.to_string() })?;
````

````
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
cargo run --bin demo

# out (relevant)
| coins toggle id
╭────┬─────────┬─────────────────────────────┬────────┬─────────────┬─────────────╮
│ id │ toggled │ timestamp                   │ person │ total_coins │ description │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 2  │ true    │ 2025-10-23 04:07:23.904 UTC │ Lan    │ 0           │ create user │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 3  │ true    │ 2025-10-23 04:07:23.904 UTC │ Lan    │ -1000       │ big debt    │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 4  │ true    │ 2025-10-23 04:09:34.976 UTC │ Lan    │ 50          │ some job    │
╰────┴─────────┴─────────────────────────────┴────────┴─────────────┴─────────────╯
enter Select event id to toggle (u32) or type [q]uit: 
3   
Toggled event enabled
| coins toggle desc
╭────┬─────────┬─────────────────────────────┬────────┬─────────────┬─────────────╮
│ id │ toggled │ timestamp                   │ person │ total_coins │ description │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 2  │ false   │ 2025-10-23 04:07:23.904 UTC │ Lan    │ 0           │ create user │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 3  │ true    │ 2025-10-23 04:07:23.904 UTC │ Lan    │ -1000       │ big debt    │
├────┼─────────┼─────────────────────────────┼────────┼─────────────┼─────────────┤
│ 4  │ false   │ 2025-10-23 04:09:34.976 UTC │ Lan    │ 50          │ some job    │
╰────┴─────────┴─────────────────────────────┴────────┴─────────────┴─────────────╯
enter Description substring or type [q]uit: 
````

2025-10-23 Wk 43 Thu - 10:22 +03:00

````diff
// in fn coin_store_toggle_by_id
let in_partial = objects_p
	.iter()
-	.any(|object_p| object_p.id == object.id);
+	.any(|object_p| object_p.ev_id == object.ev_id);
````

The IDs are constantly changing for these partial groups as they're deleted and regenerated, and the ID is incremental.

2025-10-23 Wk 43 Thu - 11:05 +03:00

I implemented all the commands, but there is an issue. When we push a new frame, it will capture events from reset lower span frames, and not the latest.

if we think to only capture the latest, then at span 50, we may need to know the latest for all spans 1..49. Or we need to freeze sourcing at the latest parent span's creation.

2025-10-23 Wk 43 Thu - 11:11 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
git commit -m "implemented demo commands"

# out
Trim Trailing Whitespace.................................................Passed
Check Yaml...........................................(no files to check)Skipped
Check for added large files..............................................Passed
Check formatting.........................................................Passed
Run tests................................................................Passed
Check clippy lints.......................................................Passed
[main a6dfa06] implemented demo commands
 5 files changed, 550 insertions(+), 154 deletions(-)
 delete mode 100644 migrations/2025-09-12-162639_create_credit_store/down.sql
 delete mode 100644 migrations/2025-09-12-162639_create_credit_store/up.sql
````

We need to fix this back at the demo in [004 Investigate options for materializing views into tables using SQL](../../000%20Implement%20the%20Event%20Accumulator/investigations/004%20Investigate%20options%20for%20materializing%20views%20into%20tables%20using%20SQL.md)

Spawn [000 Upper Span frames sources from reset lower span frames](../../000%20Implement%20the%20Event%20Accumulator/issues/000%20Upper%20Span%20frames%20sources%20from%20reset%20lower%20span%20frames.md) ^spawn-issue-4f102d

2025-10-23 Wk 43 Thu - 12:12 +03:00

Nonetheless, we have a demo here, despite the bug. Let's add some animations to the guide.md
