---
parent: '[[000 Attempting to upgrade rustyline for shi]]'
spawned_by: '[[000 Attempting to upgrade rustyline for shi]]'
context_type: issue
status: done
---

Parent: [000 Attempting to upgrade rustyline for shi](../000%20Attempting%20to%20upgrade%20rustyline%20for%20shi.md)

Spawned by: [000 Attempting to upgrade rustyline for shi](../000%20Attempting%20to%20upgrade%20rustyline%20for%20shi.md)

Spawned in: [^spawn-issue-ca4626](../000%20Attempting%20to%20upgrade%20rustyline%20for%20shi.md#spawn-issue-ca4626)

# 1 Journal

2025-09-19 Wk 38 Fri - 06:06 +03:00

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-11-upgrade-rustyline
cargo +nightly fmt --
cargo test --all-targets --all-features
````

![Pasted image 20250919060831.png](../../../../../../../../../../../../attachments/Pasted%20image%2020250919060831.png)

So we have invalid use of [`Editor`](https://github.com/kkawakam/rustyline/blob/17696602d0f4722f6d838555762f155f674dee9f/src/lib.rs#L586). Now it takes two generics arguments, a [`Helper`](https://github.com/kkawakam/rustyline/blob/17696602d0f4722f6d838555762f155f674dee9f/src/lib.rs#L547) and [`History`](https://github.com/kkawakam/rustyline/blob/17696602d0f4722f6d838555762f155f674dee9f/src/history.rs#L44).

Their [diy_hints.rs](https://github.com/kkawakam/rustyline/blob/master/examples/diy_hints.rs) hints that we can include [`DefaultHistory`](https://github.com/kkawakam/rustyline/blob/17696602d0f4722f6d838555762f155f674dee9f/src/history.rs#L640):

````rust
/// Default transient in-memory history implementation
#[cfg(not(feature = "with-file-history"))]
pub type DefaultHistory = MemHistory;
/// Default file-based history implementation
#[cfg(feature = "with-file-history")]
pub type DefaultHistory = FileHistory;
````

provided we do not use the feature `with-file-history`, we can use in-memory history with this.

2025-09-19 Wk 38 Fri - 07:00 +03:00

So this line

````rust
let mut rl = Editor::with_config(config);
````

would give us `Editor<ExecHelper<'_, S>>` in rustyline v7.1.0.

(update)

In rustyline v17.0.1, it instead returns  `Result<Editor<ExecHelper<'_, S>, FileHistory>, ReadlineError>`

2025-09-19 Wk 38 Fri - 08:31 +03:00

vscode intellisense says `{unknown}` for `ExecHelper<'_, S>` for some reason there.

(/update)

[`ReadLine -> new`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/readline.rs#L28) now can fail, so it needs to return a result.

2025-09-19 Wk 38 Fri - 07:13 +03:00

They use a crate-global error enum [`ShiError`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/error.rs#L5) that is used with

````rust
use crate::Result;
````

so we should add a new error to this.

They actually have

````rust
#[error("readline error")]
ReadlineError(#[from] rustyline::error::ReadlineError),
````

so let's reuse it.

2025-09-19 Wk 38 Fr - 07:23 +03:00i

````diff
-rl: Editor<ExecHelper<'a, S>>,
+rl: Editor<ExecHelper<'a, S>, rustyline::history::DefaultHistory>,
````

Seems happy with this, but we may need to watch for possible custom history code in here.

2025-09-19 Wk 38 Fri - 07:38 +03:00

Just one issue remains about History.

![Pasted image 20250919073842.png](../../../../../../../../../../../../attachments/Pasted%20image%2020250919073842.png)

In rustyline v7.1.0,

We could iterate over the history:

````rust
impl<'a> IntoIterator for &'a History {
    type IntoIter = Iter<'a>;
    type Item = &'a String;

    fn into_iter(self) -> Iter<'a> {
        self.iter()
    }
}
````

And they had an `.iter()`:

````rust
/// Return a forward iterator.
pub fn iter(&self) -> Iter<'_> {
	Iter(self.entries.iter())
}
````

We expect to get `String` elements.

This was removed.

In addition, `shell.rl.history()` used to return a `&History` but now it returns `&dyn History`.

2025-09-19 Wk 38 Fri - 07:53 +03:00

Also `get` used to have the signature

````rust
pub fn get(&self, index: usize) -> Option<&String>;
````

But now it has

````rust
pub fn get(&self, index: usize, dir: SearchDirection) -> Result<Option<SearchResult<'_>>>;
````

2025-09-19 Wk 38 Fri - 07:59 +03:00

We just had to collect it a bit more manually into `Vec<String>`. Getting the entry from the search result.
