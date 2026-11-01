---
parent: '[[000 Learning]]'
spawned_by: '[[000 Mn 09 Learning]]'
context_type: entry
---

Parent: [000 Learning](../000%20Learning.md)

Spawned by: [000 Mn 09 Learning](000%20Mn%2009%20Learning.md)

Spawned in: [^spawn-entry-07b270](000%20Mn%2009%20Learning.md#spawn-entry-07b270)

# Journal

2025-09-05 Wk 36 Fri - 13:48

While going through the [diesel getting-started](https://diesel.rs/guides/getting-started) I learned about [dotenvy](https://github.com/allan2/dotenvy) for managing environmental variables in Rust.

We can create an `.env`  file, which is just a collection of key-value pairs, and its content will be treated as environmental variables within Rust when we run

````rust
use dotenvy::dotenv;
use std::env;

// [...]

dotenv().ok();

let my_env_var =  env::var("MY_ENV_VAR").expect("MY_ENV_VAR must be set");
````

We just need the dependency in `Cargo.toml`:

````sh
cargo add dotenvy
````

This helps keep environmental variables local to the project, but also give the user choice in how they are specified. Since it just uses `env::var`, the user has the choice to pass environmental variables explicitly themselves, or override defaults from the `.env` file.
