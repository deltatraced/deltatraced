---
parent: '[[002 Move credit_store_demo project to deltachives]]'
spawned_by: '[[002 Move credit_store_demo project to deltachives]]'
context_type: task
status: done
---

Parent: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned by: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned in: [^spawn-task-dfb856](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md#spawn-task-dfb856)

# 1 Journal

2025-09-18 Wk 38 Thu - 20:17 +03:00

Spawn [000 Reviewing impl Trait type meaning](../investigations/000%20Reviewing%20impl%20Trait%20type%20meaning.md) ^spawn-invst-3cfcad

2025-09-18 Wk 38 Thu

2025-09-18 Wk 38 Thu - 22:12 +03:00

Trying to figure out  expected signatures for [`cmd!`](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/lib.rs#L59) macro in shi, we see it traces to `pub type BasicCommandFn<S> = Rc<dyn Fn(&mut S, &[String]) -> Result<String>>;`

which I added

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/shi
# in branch fix-clippy-lints-1
git blame src/command/basic.rs

# out (relevant)
32d2944d (Mohammed Alzakariya 2025-08-29 11:27:12 +0300  6) pub type BasicCommandFn<S> = Rc<dyn Fn(&mut S, &[String]) -> Result<String>>;
````

on clippy lints recommendation, but it awaits merging. It should be available though in my latest now.

For the [register](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/shell.rs#L108) function we need a [Command](https://github.com/Utagai/shi/blob/ef0428b1440153818ee5512adf378ba1544e0598/src/command/mod.rs#L43).

2025-09-18 Wk 38 Thu - 23:38 +03:00

Spawn [000 Internal state for shi shell not passing to new thread safely](../issues/000%20Internal%20state%20for%20shi%20shell%20not%20passing%20to%20new%20thread%20safely.md) ^spawn-issue-26db0c

2025-09-19 Wk 38 Fri - 04:14 +03:00

Spawn [001 Getting many debugging logs from rustyline while using shi](../issues/001%20Getting%20many%20debugging%20logs%20from%20rustyline%20while%20using%20shi.md) ^spawn-issue-229e4a

2025-09-19 Wk 38 Fri - 10:43 +03:00

Okay! `expt001` has a basic example of spawning a shell on its own thread with internal and external facing state. It has a simple counter function for demonstration. This should cover our needs for the shell!
