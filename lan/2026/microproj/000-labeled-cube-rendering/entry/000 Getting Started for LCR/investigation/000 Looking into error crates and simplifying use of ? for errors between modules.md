---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[002 Setup openGL rendering app in rust and do some dev]]'
context_type: investigation
status: done
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [002 Setup openGL rendering app in rust and do some dev](../task/002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md)

Spawned in: [^spawn-invst-d25645](../task/002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md#spawn-invst-d25645)

# 1 Conclusion

We ended up choosing `error_set` and to have a unified `errors.rs` file to be able to use `?` for any subset error type.

# 2 Journal

2026-05-19 Wk 21 Tue - 01:10 +03:00

It seems it was made so that you would have a single `errors.rs` with a single, project-wide error set. This makes sense in that it will be deriving all possible subset `From` implementations in resolving `error_set::error_set!`. It seems it should be possible to import into that macro, and handle all possible permutations but this time only with the new elements in the new `error_set!` imported into, but they may have avoided this complexity by instead requiring a single `error_set!`. Note that you can still source error types as usual between multiple `error_set!`s.

Let's see how `?` behaves with a repro,

````sh
# in /home/lan/src/cloned/gh/deltachives/labeled-cube-rendering-2026-m000/rs/src/repro
ls

# out
a.rs  b.rs  c.rs  mod.rs
````

````rust
// in ../../Cargo.toml
error_set = "0.9.1"

// in ../lib.rs
pub mod repro;

// in mod.rs
pub mod a;
pub mod b;
pub mod c;

// in a.rs
error_set::error_set! {
    ErrorsA := ErrorsA1 || ErrorsA2

    ErrorsA1 := {
        A11,
        A12,
        Shared1,
    }

    ErrorsA2 := {
        A21,
        A22,
        Shared1,
    }
}

pub fn compute() -> Result<(), ErrorsA> {
    Ok(())
}

// in b.rs
error_set::error_set! {
    ErrorsB := ErrorsB1 || ErrorsB2

    ErrorsB1 := {
        B11(crate::repro::a::ErrorsA),
        B12,
        Shared2,
        Shared1,
    }
}

pub fn compute() -> Result<(), ErrorsB> {
    crate::repro::a::compute()?;
    Ok(())
}

// in c.rs
error_set::error_set! {
    ErrorsC := ErrorsC1

    ErrorsC1 := {
        C11(crate::repro::b::ErrorsB),
    }
}

pub fn compute() -> Result<(), ErrorsC> {
// (error)
//    crate::repro::a::compute()?;
//								~
/*
	`?` couldn't convert the error to `ErrorsC`  
	the question mark operation (`?`) implicitly performs a conversion on the error value using the `From` trait  
	the following other types implement trait `From<T>`:  
	`ErrorsC` implements `From<ErrorsB>`  
	`ErrorsC` implements `From<ErrorsC1>` [...]
*/
// (/error)
    Ok(())
}


````

In this example, `?` works fine in `b::compute`, It behaves like `$[from]`.  If we have one `error_set!` per module, then this will at least eliminate nesting that happens through `ConsumerError -> ModuleError -> TypeInModuleError`, where we are two layers deep and `?` is not an option from `TypeInModuleError` to `ConsumerError`.

Wheras in `c::compute`, we err because we have to go through two modules, and `?` only works for the variants and the sourced ones, but no further.

And if you had a `a::compute1 : () -> Results<(), ErrorsA1>` You would not be able to use `?` on this in `b::compute` just because `ErrorsA1` is a subset of `ErrorsA` which you source in `b.rs`.  This is a disadvantage, I would have intended that sourcing an error superset entails sourcing an error subset. This seems to be an issue we can only bypass if we are willing to have a singular `errors.rs` file per project, then `ErrorsB` would simply use `||` to be a superset of `ErrorsA` which is itself a superset of `ErrorsA1`, and this would work as we expect, it would simply be an enum with no source hierarchy besides actually external packages, and this would give us the desired effect of including `ErrorsA` to mean all subsets of it. The apparent disadvantage of course is that a single `errors.rs` file can be unwieldy as a project scales. And it exposes all levels of abstraction at once, wheras otherwise modules would only see, mention, or express things at their level of abstraction only.

Note this was also an issue for us with `thiserror`, that the consumed module would expose functions using various error types, and as a consumer we only have the error variant be a superset of those. We have to convert to the superset first before we can do `?`. So either way, we are paying this cost with different error types distributed across modules.

2026-05-19 Wk 21 Tue - 03:01 +03:00

For us in this project, let us choose a singular `errors.rs` to get full subset $\to$ superset error conversion support. We can still tag each respective file. This should mean that we need to strive to make this file easy to import by all files in the repository. It's a central node.
