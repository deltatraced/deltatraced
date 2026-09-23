---
parent: '[[000 Learning]]'
spawned_by: '[[000 Learning]]'
context_type: entry
---

Parent: [000 Learning](../000%20Learning.md)

Spawned by: [000 Learning](../000%20Learning.md)

Spawned in: [^spawn-entry-ddd17e](../000%20Learning.md#spawn-entry-ddd17e)

[Mn 10 October](../../../../../2026-05-21_2025/main/overview/monthly/2025/Mn%2010%20October.md)

# 1 Purpose

Capturing highlights of practices and lessons learned this month!

# 2 Journal

2025-10-09 Wk 41 Thu - 19:15 +03:00

(1)

We can use windows in Rust iterators to process a moving window of values with tuple size up to 12!

Here's an example from dbmint:

````rust
let settings_of_the_same_table_are_consecutive = settings
	.iter()
	.enumerate()
	.chunk_by(|(_, k)| {
		match k {
			Setting::TableDeriveEventSourcing(setting) => &setting.table_line.table_name,
			Setting::ColumnESSetting(setting) => &setting.table_line.table_name,
		}
	})
	.into_iter()
	.all(|(_, group)| {
		group
			.into_iter()
			.tuple_windows::<(_, _)>()
			.all(|((i0, _), (i1, _))| i1 - i0 == 1)
	});
````

In order to prove consecutive indices, I have to check that any two indices only differ by 1. Using `tuple_windows` we can do that. A moving window should always include two values to do the comparison moving through the iterator. If the iterator has less than 2 elements, then no windows are produced.

We're also using `chunk_by`! All items having the same key end up in the same group!

2025-10-16 Wk 42 Thu - 10:08 +03:00

(2)

During [001 Reading through lukaswirth.dev decl-macros](../../../../../2026-05-21_2025/microproj/001%20Rust%20Diesel%20Event%20Sourcing/tasks/2025/000%20Implement%20the%20Event%20Accumulator/entries/001%20Reading%20through%20lukaswirth.dev%20decl-macros.md),

I have been thinking about specification writing and the this [rust reference point](https://doc.rust-lang.org/reference/macros-by-example.html#r-macro.decl.transcription.fragment) provides a good example of how specification should be in a sense executable. They provide specific examples to mark exceptions or invariants. This can be a way that tests and specification interact, with tests providing stimulus, ie. concrete examples to demonstrate expected errors and behaviors. The spec then can, in natural language, provide a minimal reproduction of that example to illustrate its logical point.

I also incorporate errors in the types of the functions in Rust. Some errors are due to the function input domain varying in unintended ways and are meant to be handled externally. Others are Invariant Errors, which should never trigger in a correct implementation, but yet are technically possible given the compiler context.

Wherever we can, invariants should be encoded through computation by constructing new types whose values are always valid. This can help to write better tests, but it also helps clarify how our software can fail, which goes back to spec writing as well.

2025-10-26 Wk 43 Sun - 19:42 +03:00

(3)

We can use variables in vim search and replace or with sed! All with the power of capture groups!

Let's say we want to fix this text down here: We want all the hexadecimal values that are not 2-padded with zero to be so.

````
.byte 0x0, 0x10, 0x3, 0x8
.byte 0x0, 0xF0, 0x0, 0x6
````

This sadly doesn't work with obsidian vim mode as of this writing, but it works fine with `vim` or with `sed`:

````
:'<,'>s/0x\([0-9A-F]\)\(,\|$\)/0x0\1\2/g
````

This finds the pattern `0x` followed by a capture group for a hexadecimal value, followed by an optional comma capture group (`,\|$` means comma or end of line), and when applied to the selection gives us

````
.byte 0x00, 0x10, 0x03, 0x08
.byte 0x00, 0xF0, 0x00, 0x06
````

All padded!

What if we want to turn them to `.word` u32 directives instead? we have to merge all the bytes into a single word!

````
:'<,'>s/.byte 0x\([0-9A-F][0-9A-F]\), 0x\([0-9A-F][0-9A-F]\), 0x\([0-9A-F][0-9A-F]\), 0x\([0-9A-F][0-9A-F]\)/.word 0x\4\3\2\1/g
````

````
.word 0x08031000
.word 0x0600F000
````

Now they're merged!

There's so many use cases for this where we want to capture part of the search pattern as a variable in our replace!
