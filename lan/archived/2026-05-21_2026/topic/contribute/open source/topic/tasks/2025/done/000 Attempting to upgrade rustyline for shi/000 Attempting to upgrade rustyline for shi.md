---
status: done
---

# 1 Journal

2025-09-19 Wk 38 Fri - 05:21 +03:00

Related to [001 Getting many debugging logs from rustyline while using shi](../../../../../../../../../2026-05-21_2025/microproj/001%20Rust%20Diesel%20Event%20Sourcing/tasks/2025/002%20Move%20credit_store_demo%20project%20to%20deltachives/issues/001%20Getting%20many%20debugging%20logs%20from%20rustyline%20while%20using%20shi.md)

Issue is in [gh Utagai/shi #11](https://github.com/Utagai/shi/issues/11).

2025-09-19 Wk 38 Fri - 05:23 +03:00

Spawn [000 Prepare new branch for shi rustyline upgrade](tasks/000%20Prepare%20new%20branch%20for%20shi%20rustyline%20upgrade.md) ^spawn-task-8c0363

2025-09-19 Wk 38 Fri - 05:56 +03:00

So after trying to bump versions to

````toml
rustyline = "17.0.1"
rustyline-derive = "0.11.1"
````

2025-09-19 Wk 38 Fri - 06:06 +03:00

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-11-upgrade-rustyline
cargo +nightly fmt --
cargo test --all-targets --all-features
````

Spawn [000 rustyline now includes a new History param for shi](issues/000%20rustyline%20now%20includes%20a%20new%20History%20param%20for%20shi.md) ^spawn-issue-ca4626

2025-09-19 Wk 38 Fri - 08:00 +03:00

build is OK, and tests all pass. Let's do some more manual testing, checking that shi functionality is still the same.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-11-upgrade-rustyline
cargo run --example simple

# out (relevant)
| A
| B
| C
| history
        A
        B
        C
        history
````

We are also able to press up and down to find our commands `A`, `B`, `C`, and `history`.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-clippy-lints-1
cargo run --example simple
````

This gives us the same behavior as above.

2025-09-19 Wk 38 Fri - 08:04 +03:00

Okay, this looks good. README.md example and all examples are updated, since creating a shell now can also fail, we add an extra `?`.

````diff
-let mut shell = Shell::new("| ");
+let mut shell = Shell::new("| ")?;

-let mut shell = Shell::new_with_state("| ", counter);
+let mut shell = Shell::new_with_state("| ", counter)?;
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-11-upgrade-rustyline
cargo +nightly fmt --
cargo test --all-targets --all-features

# out (relevant)
[OK]
````

2025-09-19 Wk 38 Fri - 08:07 +03:00

Interesting. if we try to commit with no `pre-commit-config.yaml`  but pre-commit installed we get

````
No .pre-commit-config.yaml file was found
- To temporarily silence this, run `PRE_COMMIT_ALLOW_NO_CONFIG=1 git ...`
- To permanently silence this, install pre-commit with the --allow-missing-config option
- To uninstall pre-commit run `pre-commit uninstall`
````

Let's uninstall it for now on this PR until it's merged.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-11-upgrade-rustyline
pre-commit uninstall

# out
pre-commit uninstalled
````

2025-09-19 Wk 38 Fri - 08:16 +03:00

We opened a PR! [gh Utagai/shi #12](https://github.com/Utagai/shi/pull/12).
