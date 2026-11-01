---
parent: '[[000 Attempting to upgrade rustyline for shi]]'
spawned_by: '[[000 Attempting to upgrade rustyline for shi]]'
context_type: task
status: done
---

Parent: [000 Attempting to upgrade rustyline for shi](../000%20Attempting%20to%20upgrade%20rustyline%20for%20shi.md)

Spawned by: [000 Attempting to upgrade rustyline for shi](../000%20Attempting%20to%20upgrade%20rustyline%20for%20shi.md)

Spawned in: [^spawn-task-8c0363](../000%20Attempting%20to%20upgrade%20rustyline%20for%20shi.md#spawn-task-8c0363)

# 1 Journal

2025-09-19 Wk 38 Fri - 05:26 +03:00

We need to make sure that our master is in sync with current shi master. We currently have unmerged changed from PR [gh Utagai/shi #10](https://github.com/Utagai/shi/pull/10).

![Pasted image 20250919052837.png](../../../../../../../../../../../../attachments/Pasted%20image%2020250919052837.png)

We probably should in the future just keep an `unmerged` branch for anything in the fork that's ahead and currently unmerged, so that main or master remain in sync for us to start new features. Also because unmerged is likely to be different once the merge is approved.

The latest commit that is merged is my commit [ef0428b](https://github.com/Utagai/shi/commit/ef0428b1440153818ee5512adf378ba1544e0598). Let's reset our master back to this.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/shi
# in branch master
git reset --hard ef0428b
git push origin master --force
git checkout -b fix-11-upgrade-rustyline
````

Issue now is there will be linting errors due to unmerged [gh Utagai/shi #10](https://github.com/Utagai/shi/pull/10).

We shouldn't try to fix them until it is merged. We need to only complete our objective of upgrading the dependency and making sure build, tests, and styling pass ourselves.

2025-09-19 Wk 38 Fri - 05:47 +03:00

Use these:

````sh
cargo +nightly fmt --
cargo test --all-targets --all-features
````

2025-09-19 Wk 38 Fri - 06:23 +03:00

We also moved the branches to

````
/home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-11-upgrade-rustyline
/home/lan/src/cloned/gh/LanHikari22/forked/Utagai/branches/shi@fix-clippy-lints-1
````

so that we can make simultaneous changes or compare differences quickly between them.
