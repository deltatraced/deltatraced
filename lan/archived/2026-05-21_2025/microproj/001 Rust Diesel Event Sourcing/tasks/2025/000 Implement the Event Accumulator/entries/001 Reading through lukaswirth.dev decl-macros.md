---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[006 Using macro_rules to automate event and hist creation]]'
context_type: entry
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [006 Using macro_rules to automate event and hist creation](../tasks/006%20Using%20macro_rules%20to%20automate%20event%20and%20hist%20creation.md)

Spawned in: [^spawn-entry-4ac46d](../tasks/006%20Using%20macro_rules%20to%20automate%20event%20and%20hist%20creation.md#spawn-entry-4ac46d)

# 1 Journal

2025-10-16 Wk 42 Thu - 09:25 +03:00

[lukaswirth.dev decl-macros](https://lukaswirth.dev/tlborm/decl-macros.html).

Creating an experiment for this in [gh LanHikari22/rs_repro](https://github.com/LanHikari22/rs_repro): `expt001_4ac46d_macro_rules.rs`

Renamed `expt000_howto_ecf5e1.rs` $\to$ `expt000_ecf5e1_stateful_iter.rs`

2025-10-16 Wk 42 Thu - 09:38 +03:00

[Rust Ref Macros-By-Example](https://doc.rust-lang.org/reference/macros-by-example.html) the `macro_rules!` system is called Macros-By-Example or MBE for short as the author of [lukaswirth.dev decl-macros](https://lukaswirth.dev/tlborm/decl-macros.html) refers to it.

2025-10-16 Wk 42 Thu - 19:09 +03:00

Given

````rust
macro_rules! macro000 {
    {3} => {};
}

fn main() {
    macro000!(3);
}
````

Yields

````sh
# in /home/lan/src/cloned/gh/LanHikari22/rs_repro
cargo +nightly expand --bin expt001_4ac46d_macro_rules

# out (relevant)
#![feature(prelude_import)]
#[macro_use]
extern crate std;
#[prelude_import]
use std::prelude::rust_2024::*;
fn main() {}
````

2025-10-16 Wk 42 Thu - 19:46 +03:00

````rust
error: expected one of `.`, `;`, `?`, `}`, or an operator, found `expr` metavariable
  --> src/bin/expt001_4ac46d_macro_rules.rs:8:30
   |
8  |       {$($name:expr),*;} => {$($name)*;}
   |                                ^^^^^ expected one of `.`, `;`, `?`, `}`, or an operator
...
14 | /     macro001! {
15 | |         "aaa",
16 | |         "bbb", "ccc";
17 | |     }
   | |_____- in this macro invocation
   |
   = note: this error originates in the macro `macro001` (in Nightly builds, run with -Z macro-backtrace for more info)
````

Why?

2025-10-16 Wk 42 Thu - 19:54 +03:00

Sources checked: [stackoverflow answer 1](https://stackoverflow.com/a/73282970/6944447), [lukaswirth.dev decl-macros repetitions](https://lukaswirth.dev/tlborm/decl-macros/macros-methodical.html#repetitions)

2025-10-16 Wk 42 Thu - 19:58 +03:00

````rust
macro_rules! macro001 {
    {$($name:expr),*} => {
        $(
            println!("{}", $name);
        )*
    }
}
fn main() {
    macro001! {
        "aaa",
        "bbb", "ccc"
    }
}
````

Yields

````sh
# in /home/lan/src/cloned/gh/LanHikari22/rs_repro
cargo +nightly expand --bin expt001_4ac46d_macro_rules

# out (relevant)
#![feature(prelude_import)]
//! Experiment in delta-trace
#[macro_use]
extern crate std;
#[prelude_import]
use std::prelude::rust_2024::*;
fn main() {
    {
        ::std::io::_print(format_args!("{0}\n", "aaa"));
    };
    {
        ::std::io::_print(format_args!("{0}\n", "bbb"));
    };
    {
        ::std::io::_print(format_args!("{0}\n", "ccc"));
    };
}
````

So it was an issue with usage.

Anyway we should allow optional terminating commas as with [stackoverflow answer 1](https://stackoverflow.com/a/73282970/6944447),

````rust
macro_rules! macro001 {
    {$($name:expr),+ $(,)?} => {
        $(
            println!("{}", $name);
        )*
    }
}


fn main() {
    macro001! {
        "aaa",
        "bbb", "ccc"
    }
}
````

yields

````sh
# in /home/lan/src/cloned/gh/LanHikari22/rs_repro
cargo +nightly expand --bin expt001_4ac46d_macro_rules

# out (relevant)
#![feature(prelude_import)]
//! Experiment in delta-trace
#[macro_use]
extern crate std;
#[prelude_import]
use std::prelude::rust_2024::*;
fn main() {
    {
        ::std::io::_print(format_args!("{0}\n", "aaa"));
    };
    {
        ::std::io::_print(format_args!("{0}\n", "bbb"));
    };
    {
        ::std::io::_print(format_args!("{0}\n", "ccc"));
    };
}
````

But we can also use a trailing comma:

````rust
macro_rules! macro001 {
    {$($name:expr),+ $(,)?} => {
        $(
            println!("{}", $name);
        )*
    }
}


fn main() {
    macro001! {
        "aaa", "bbb", "ccc",
    }
}
````

yields

````sh
# in /home/lan/src/cloned/gh/LanHikari22/rs_repro
cargo +nightly expand --bin expt001_4ac46d_macro_rules

# out (relevant)
#![feature(prelude_import)]
//! Experiment in delta-trace
#[macro_use]
extern crate std;
#[prelude_import]
use std::prelude::rust_2024::*;
fn main() {
    {
        ::std::io::_print(format_args!("{0}\n", "aaa"));
    };
    {
        ::std::io::_print(format_args!("{0}\n", "bbb"));
    };
    {
        ::std::io::_print(format_args!("{0}\n", "ccc"));
    };
}
````

Which is the same.

2025-10-16 Wk 42 Thu - 20:19 +03:00

Similar to [stackoverflow answer](https://stackoverflow.com/questions/25877285/how-do-you-disable-dead-code-warnings-at-the-crate-level-in-rust#25877389), we can do `#![allow(unexpected_cfgs)]` but we're not able to pass local features in.

2025-10-16 Wk 42 Thu - 20:26 +03:00

Let's just pass an input. Similar to `/home/lan/src/cloned/gh/deltachives/2025-Wk37-000-obsidian-migration/src/bin/expt000_parse_single_pulldown_cmark.rs`,

````rust
use std::env::args;

fn main() {
	let choice = args()
		.nth(1)
		.expect("Must provide a choice")
}
````

Actually no need for runtime, simply seperating the macro uses into their own functions like `use_macro000`, we're able to just filter out all the others in output.

2025-10-16 Wk 42 Thu - 20:46 +03:00

For our db automation needs, we can use something similar to `macro002`:

````rust
macro_rules! macro002 {
    {$($name:ident: $typ:ty),+ $(,)?} => {
        pub struct Macro002<'a> {
            $(
                pub $name: $typ,
            )*
        }
    }
}

fn use_macro002() {
    /*
        cargo +nightly expand --bin expt001_4ac46d_macro_rules

        # out (relevant)

        #![feature(prelude_import)]
        //! Experiment in delta-trace
        #[macro_use]
        extern crate std;
        #[prelude_import]
        use std::prelude::rust_2024::*;
        fn use_macro002() {
            pub struct Macro002<'a> {
                pub _person: &'a str,
                pub _coins: i32,
            }
            let _ = Macro002 { _person: "", _coins: 0 };
        }
    */
    macro002! {
        _person: &'a str,
        _coins: i32,
    }

    let _ = Macro002 { _person: "", _coins: 0 };
}
````

2025-10-17 Wk 42 Fri - 11:15 +03:00

There's a different form. We only need the table portion to be able to create {Table}Diff, {Table}Events, {Table}Hist, etc.

This is related to [stackoverflow post 39017871](https://stackoverflow.com/questions/39017871/how-to-prefix-suffix-identifiers-within-a-macro#41784145).

This seems difficult to do with `macro_rules!`.  Let's follow the suggestion of using a mod instead.
