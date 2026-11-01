---
parent: '[[002 Move credit_store_demo project to deltachives]]'
spawned_by: '[[000 Modularize shi shell use in credit store demo]]'
context_type: issue
status: done
---

Parent: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned by: [000 Modularize shi shell use in credit store demo](../tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md)

Spawned in: [^spawn-issue-26db0c](../tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md#spawn-issue-26db0c)

# 1 Journal

2025-09-18 Wk 38 Thu - 23:39 +03:00

We're trying to spawn a thread for the shell to run commands from. Right now we pass some internal state `S` with an associated lifetime `'a` that corresponds to the shell lifetime. This worked for `create_shell`, but not for passing it to the a thread.

````
error[E0277]: `S` cannot be sent between threads safely
   --> src/drivers/shell.rs:54:19
    |
54  |       thread::spawn(move || {
    |       ------------- ^------
    |       |             |
    |  _____|_____________within this `{closure@src/drivers/shell.rs:54:19: 54:26}`
    | |     |
    | |     required by a bound introduced by this call
55  | |         let mut mut_shell = create_shell(shell_state, commands)
56  | |             .map_err(SpawnShellLoopThreadError::CreateShellError)?;
...   |
84  | |         Ok(())
85  | |     })
    | |_____^ `S` cannot be sent between threads safely
    |
note: required because it's used within this closure
   --> src/drivers/shell.rs:54:19
    |
54  |     thread::spawn(move || {
    |                   ^^^^^^^
note: required by a bound in `spawn`
   --> /home/lan/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:726:8
    |
723 | pub fn spawn<F, T>(f: F) -> JoinHandle<T>
    |        ----- required by a bound in this function
...
726 |     F: Send + 'static,
    |        ^^^^ required by this bound in `spawn`
help: consider further restricting type parameter `S` with trait `Send`
    |
53  | fn spawn_shell_loop_thread<'a, S: 'a + std::marker::Send>(shell_state: S, commands: Vec<Command<'a, S>>) -> JoinHandle<Result<(), SpawnShellLoopThreadError>> {
    |                                      +++++++++++++++++++
````

2025-09-19 Wk 38 Fri - 03:57 +03:00

We can mark to the compiler it's safe with `+ Send + 'static`. That seems to work fine

I opted to also create those values inside the thread since they do not need to be shared across the thread.
