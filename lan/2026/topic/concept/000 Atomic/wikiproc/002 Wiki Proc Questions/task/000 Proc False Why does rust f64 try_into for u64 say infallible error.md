---
context_type: task
status: done
tags:
- rust
---

Parent: [002 Wiki Proc Questions](../002%20Wiki%20Proc%20Questions.md)

Spawned by: [002 Wiki Proc Questions](../002%20Wiki%20Proc%20Questions.md)

Spawned in: [^spawn-task-46dbb3](../002%20Wiki%20Proc%20Questions.md#spawn-task-46dbb3)

Wiki: [000 False Why does rust f64 try_into for u64 say infallible error](../../../wiki/002%20Wiki%20Questions/task/000%20False%20Why%20does%20rust%20f64%20try_into%20for%20u64%20say%20infallible%20error.md)

# Journal

2026-05-26 Wk 22 Tue - 07:53 +03:00

Encountered this trying to convert `JsValue` to `f64` to `u64`, and I expected there to be an error, since `1.45f64` is a valid `f64` value but not a `u64` value. but it says it is `Infallible`, so do I have the wrong expectation?

I think the issue is that `From<T>` and inverses like `Into<T>`, and variants that can fail like `TryInto<T>`, `TryFrom<T>`, are about direct equivalences. f64 and u64 simply are *not* equivalent. But 3u32 and 3u64 are equivalent for example.

At the same time, f64 and u64 *are* equivalent when you only care about f64 as a binary string rather than as an actual floating point number, so the conversion is infallible.

2026-05-26 Wk 22 Tue - 08:08 +03:00

The question was renamed `Invalid` because it does not express a true proposition. The fact is, there is no `From<f64>` for `u64`. But when you use `?` you get this error message:

````
error[E0277]: the trait bound `Result<u64, _>: TryFrom<f64>` is not satisfied
  --> src/silverbullet_syscalls/util.rs:40:31
   |
40 |     let b: Result<u64, _> = a.try_into()?;
   |                               ^^^^^^^^ the trait `From<f64>` is not implemented for `Result<u64, _>`
   |
   = help: the trait `From<PromiseState<T>>` is implemented for `Result<T, wasm_bindgen::JsValue>`
   = note: required for `f64` to implement `Into<Result<u64, _>>`
   = note: required for `Result<u64, _>` to implement `TryFrom<f64>`
   = note: required for `f64` to implement `TryInto<Result<u64, _>>`
````

and above it in vscode, it says:

````
this can't be annotated with `?` because it has type `Result<_, Infallible>`rustc[E0277](https://doc.rust-lang.org/error-index.html#E0277)

util.rs(40, 41): original diagnostic
````

And I was confused with the `Result<_, Infallible>`, interpreting it as if there does exist a non-failing conversion.
