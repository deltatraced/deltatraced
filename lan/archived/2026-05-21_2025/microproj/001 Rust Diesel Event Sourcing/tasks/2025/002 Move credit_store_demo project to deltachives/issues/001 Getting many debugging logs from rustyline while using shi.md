---
parent: '[[002 Move credit_store_demo project to deltachives]]'
spawned_by: '[[000 Modularize shi shell use in credit store demo]]'
context_type: issue
status: done
---

Parent: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned by: [000 Modularize shi shell use in credit store demo](../tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md)

Spawned in: [^spawn-issue-229e4a](../tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md#spawn-issue-229e4a)

# 1 Journal

So I enabled trace logs:

````rust
drivers::logging::init_logging_with_level(log::LevelFilter::Trace);
````

But now I'm getting many debug logs while typing each character from rustyline:

````
a[2025-09-19 04:16:18 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-7.1.0/src/tty/unix.rs:649] key: KeyEvent(Char('b'), NONE)
[2025-09-19 04:16:18 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-7.1.0/src/keymap.rs:584] Emacs command: SelfInsert(1, 'b')
[2025-09-19 04:16:18 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-7.1.0/src/undo.rs:151] Changeset::insert(1, 'b')
b[2025-09-19 04:16:19 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-7.1.0/src/tty/unix.rs:649] key: KeyEvent(Char('c'), NONE)
[2025-09-19 04:16:19 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-7.1.0/src/keymap.rs:584] Emacs command: SelfInsert(1, 'c')
[2025-09-19 04:16:19 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-7.1.0/src/undo.rs:151] Changeset::insert(2, 'c')
c
````

2025-09-19 Wk 38 Fri - 04:31 +03:00

Spawn [000 Can eliminate all logging calls with cargo feature](../ideas/000%20Can%20eliminate%20all%20logging%20calls%20with%20cargo%20feature.md) ^spawn-idea-732f18

2025-09-19 Wk 38 Fri - 04:42 +03:00

This [rust forum post](https://users.rust-lang.org/t/how-to-exclude-a-library-log-in-tracing-subsciber/121106/2) discusses that but for a [docs.rs tracing_subscriber](https://docs.rs/tracing-subscriber/latest/tracing_subscriber/index.html) crate.

And this [reddit post](https://www.reddit.com/r/rust/comments/85qp50/how_to_disable_logging_for_certain_crates/) has discussion on using various other logging crates to solve the issue... Like [gh rust-cli/env_logger](https://github.com/rust-cli/env_logger) which can be configured via environment variables.

2025-09-19 Wk 38 Fri - 05:15 +03:00

Should this library even be emitting these debug messages? I filed an issue [gh Utagai/shi #11](https://github.com/Utagai/shi/issues/11) noting the old versions of [rustyline](https://github.com/kkawakam/rustyline).

Let's attempt to upgrade. This involves breaking change.

See [000 Attempting to upgrade rustyline for shi](../../../../../../../2026-05-21_2026/topic/contribute/open%20source/topic/tasks/2025/done/000%20Attempting%20to%20upgrade%20rustyline%20for%20shi/000%20Attempting%20to%20upgrade%20rustyline%20for%20shi.md)

2025-09-19 Wk 38 Fri - 09:37 +03:00

Let's get my `unmerged` branch for shi and see if the logs go away.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@unmerged
git tag -a "0.1.6-unmerged" -m "Lan's fork" 
git push origin --tags
````

````toml
shi = { git = "https://github.com/LanHikari22/shi", version = "0.1.6-unmerged" }
````

2025-09-19 Wk 38 Fr - 09:53 +03:00i

From [cargo reference specifying dependencies](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html),

We see we need to specify `tag` instead of `version`. Though we could have specified `branch` too.

````toml
shi = { git = "https://github.com/LanHikari22/shi", tag = "0.1.6-unmerged" }
````

2025-09-19 Wk 38 Fri - 09:56 +03:00

We actually can do

````rust
#[error("Got ShiError: {0:?}")]
ShiError(#[from] ShiError),
````

And then we can do `?` on a `ShiError` directly! No need to map in the case it's a one to one correspondence.

Added to [000 Mn 09 Learning](../../../../../../../2026-05-21-pre/entries-monthly/2025/000%20Learning/entries/000%20Mn%2009%20Learning.md) ^learning-6c9d3b

2025-09-19 Wk 38 Fri - 09:57 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
cargo run --bin expt001_basic_shell

# out (relevant)
a[2025-09-19 09:57:09 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-17.0.1/src/tty/unix.rs:842] c: 'b' => key: KeyEvent(Char('b'), Modifiers(0x0))
[2025-09-19 09:57:09 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-17.0.1/src/keymap.rs:661] Emacs command: SelfInsert(1, 'b')
[2025-09-19 09:57:09 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-17.0.1/src/undo.rs:148] Changeset::insert(1, 'b')
b[2025-09-19 09:57:10 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-17.0.1/src/tty/unix.rs:842] c: 'c' => key: KeyEvent(Char('c'), Modifiers(0x0))
[2025-09-19 09:57:10 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-17.0.1/src/keymap.rs:661] Emacs command: SelfInsert(1, 'c')
[2025-09-19 09:57:10 DEBUG /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustyline-17.0.1/src/undo.rs:148] Changeset::insert(2, 'c')
c
````

Our situation did not improve!

2025-09-19 Wk 38 Fri - 10:14 +03:00

We're using [gh LOSEARDES77/fstdout-logger](https://github.com/LOSEARDES77/fstdout-logger).

Let's try [docs.rs env_logger](https://docs.rs/env_logger/latest/env_logger/) ([gh rust-cli/env_logger](https://github.com/rust-cli/env_logger)). It should have capability to filter out target logs.

````sh
cargo add env_logger

# out (relevant)
 Features:
 + auto-color
 + color
 + humantime
 + regex
 - kv
 - unstable-kv
````

2025-09-19 Wk 38 Fri - 10:25 +03:00

Okay we're able to filter the module logs out with

````rust
env_logger::builder()
	.filter_level(level)
	.filter_module("rustyline", LevelFilter::Warn)
	.try_init()
	.map_err(|e| e.to_string())
	.expect("Failed to initialize logger");
````
