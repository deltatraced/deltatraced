---
context_type: judgment
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/003 Create a Rust FFI for Silverbullet plug api](../task/003%20Create%20a%20Rust%20FFI%20for%20Silverbullet%20plug%20api.md)

Spawned in: [^spawn-jdgmt-3ba711](../task/003%20Create%20a%20Rust%20FFI%20for%20Silverbullet%20plug%20api.md#spawn-jdgmt-3ba711)

# Decision

* We will no longer use any `InternalError` variants. Those will be panics and exceptions.
* We will not document (to the module consumer) effects that should only happen if internal invariants are violated.

# Journal

2026-05-31 Wk 23 Sun - 12:05 +03:00

Prior, I had `ModuleInternalError`, but upon feedback I see that I did not give a strong argument for this. This is exactly where your library should simply panic, since you are promising that this error is triggered *only* when you make a false assumption or have a bug. So I’ll remove all `ModuleInternalError`s. It was a way for me to document most of my application’s failure modes, and then categorize which are invariant and which are due to the user. I will panic instead, and the library can still be grepped for these panics to study its invariants.

This post also captures the exact distinction: https://burntsushi.net/unwrap/, where `panic`ing is precisely to signal that the fault for a runtime error lies with the library, and an invariant has been invalidated (which is different from expected failures due to bad input)

https://nrc.github.io/error-docs/rust-errors/panic.html also mentions that panics are for errors which should not happen in theory.

In `split_at` in `/home/lan/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/str/mod.rs` , they have an example of documenting panics, but this is the kind of panic that is reflected in possible bad input state. We can make a panicing and non-panicing variants of functions if we want this pattern of giving the user a choice, but this is different from our internal errors. The internal errors are invariants, they are expected not to fail for *any* input.

To document that we may panic (never; only if we are buggy) introduces a similar internals leak to the `InternalResult`, although in documentation rather than in code.

The code should hopefully be self-documenting in how it panics, and we can of course put comments internal to the function for developers interested in the internal conditions.

I did have in mind the idea that I wanted my functions to generally never panic; this is of course still true. I should never trigger panic *unless* I am confident I cannot.

In our current case with this library, I don’t know yet, I did not test the FFI interface fully, but we really shouldn’t be panicing just from converting data structures back and forth; this is not something I want the expose to the library consumer. I exposed it as an InternalError to signal that it’s not their responsibility, but now we’re just gonna panic to really take the blame for it.

If an application critically must not panic, it needs to assume everything can panic. There is no type-guarantee for non-panicking, so we are assuming panic-as-bug by default, because, of course, code is always possibly buggy, unless we are able to make all invalid state unrepresentable and prove it in compilation.

I will use `.expect("invariant")` as shorthand, rather than `.unwrap()`. This is to prevent misunderstanding. People can read the code and expect that it is unwrapping where it shouldn’t. To label it invariant is the minimum signal to say, actually, this is expected *not* to fail.

Or we could use `inv` for invariant. `.expect(“invariant”`) is longer than `.pipe(inv)`. 21 characters vs 10! Or just `inv(T)`, so we only add 5 letters. Nothing beats the `?` at 1 character, though.

2026-05-31 Wk 23 Sun - 14:17 +03:00

Anyway in short, giving a consumer an “I failed” result can result in blame-shifting. They have no reasonable way to recover, and if they panic in your stead, it seems as if they were wrong, not your code. So for good governance, you should be the one to panic. If an application wants to assume no panics, it needs to assume full correctness of its dependencies, or otherwise assume all of it can fail and have a general recovery plan for irrecoverable source code, but we shouldn’t mix recoverable with irrecoverable, Results are recoverable.

2026-05-31 Wk 23 Sun - 14:40 +03:00

Other feedback I got also is to use `new` instead of `create` for constructors. I suspected this might be used for `Box<T>` values (heap use), but I think it is just in general.

2026-06-01 Wk 23 Mon - 05:58 +03:00

The very long error enum names can make for unpleasant API too, although `MyError::MyErrorTheErrorName` ensures uniqueness, it’s best to deal with conflicts when they come, which is just in the event that an error and another intersect *and* have an identical name but different intent/values, this is a rather rare event, so just handle it if it comes instead, though if it’s generic, like `Error`, then prefer to have the name of the module. This case might be more likely, but it is less so now that we don’t have `InternalError`s which aggregate a lot of errors.
