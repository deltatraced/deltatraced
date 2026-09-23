---
parent: '[[002 Move credit_store_demo project to deltachives]]'
spawned_by: '[[000 Modularize shi shell use in credit store demo]]'
context_type: investigation
status: done
---

Parent: [002 Move credit_store_demo project to deltachives](../002%20Move%20credit_store_demo%20project%20to%20deltachives.md)

Spawned by: [000 Modularize shi shell use in credit store demo](../tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md)

Spawned in: [^spawn-invst-3cfcad](../tasks/000%20Modularize%20shi%20shell%20use%20in%20credit%20store%20demo.md#spawn-invst-3cfcad)

# 1 Journal

2025-09-18 Wk 38 Thu - 21:26 +03:00

[Rust Reference impl Trait](https://doc.rust-lang.org/reference/types/impl-trait.html?highlight=impl%20Trait#impl-trait)

2025-09-18 Wk 38 Thu - 20:30 +03:00

From [rust-by-example impl Trait](https://doc.rust-lang.org/stable/rust-by-example/trait/impl_trait.html),

2025-09-18 Wk 38 Thu - 20:37 +03:00

For arguments,

2025-09-18 Wk 38 Thu - 21:16 +03:00

We created an experiment file `expt000` to capture some ideas here.

````rust
fn main() {
    let person = Person {};

    let cat = Cat {};

    conversate(&person);

    conversate::<Cat>(&cat);

    discuss(&person);

    discuss(&cat);
}

// out
That search engine is your friend! Just go ask it about Food.
Meow!
That search engine is your friend! Just go ask it about Art.
???
````

These are the two signatures:

````rust
fn discuss(talker: &impl Talk)
fn conversate<T: Talk>(talker: &T)
````

They perform the same function, but `conversate` in this case is more versatile as it can be specialized to the exact implementation via `conversate::<Cat>` for example. It also gives us all type information right in call site, which is useful.

![Pasted image 20250918211939.png](../../../../../../../../../attachments/Pasted%20image%2020250918211939.png)

![Pasted image 20250918212407.png](../../../../../../../../../attachments/Pasted%20image%2020250918212407.png)

This information is lost with `impl Talk`, so prefer to use [generic parameters](https://doc.rust-lang.org/reference/items/generics.html).

2025-09-18 Wk 38 Thu - 20:42 +03:00

There are some discussions on how to name traits: [gh rust-lang/api-guidelines discussion #28 ](https://github.com/rust-lang/api-guidelines/discussions/28) where the preference is imperative names.

We should strive to have one-function traits, and they're basically the CamelCase name of that that function.

2025-09-18 Wk 38 Thu - 21:25 +03:00

As explained in the [reference](https://doc.rust-lang.org/reference/types/impl-trait.html?highlight=impl%20Trait#r-type.impl-trait.return.constraint-body), `impl trait` returns allow us to save on performance penalties that would otherwise get with returning a boxed trait object. For example it's particularly useful when working with closures.

2025-09-18 Wk 38 Thu - 22:52 +03:00

In my context,

I'm getting this error:

````rust
error[E0790]: cannot call associated function on trait without specifying the corresponding `impl` type
  --> src/drivers/shell.rs:34:23
   |
15 |     fn init_shell_state() -> Result<Self::Out, InitShellStateError>;
   |     ---------------------------------------------------------------- `InternalShellState::init_shell_state` defined here
...
34 |     let shell_state = InternalShellState::init_shell_state()
   |                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ cannot call associated function of trait
````

2025-09-18 Wk 38 Thu - 23:14 +03:00

This is reproduced in `expt000`,

![Pasted image 20250918231512.png](../../../../../../../../../attachments/Pasted%20image%2020250918231512.png)
