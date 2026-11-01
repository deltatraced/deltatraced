---
context_type: task
status: done
---

Parent: [002 Wiki Questions](../002%20Wiki%20Questions.md)

Spawned by: [002 Wiki Proc Questions](../../../wikiproc/002%20Wiki%20Proc%20Questions/002%20Wiki%20Proc%20Questions.md)

Spawned in: [^spawn-task-e97f27](../../../wikiproc/002%20Wiki%20Proc%20Questions/002%20Wiki%20Proc%20Questions.md#spawn-task-e97f27)

Process Notes: [000 Proc False Why does rust f64 try_into for u64 say infallible error](../../../wikiproc/002%20Wiki%20Proc%20Questions/task/000%20Proc%20False%20Why%20does%20rust%20f64%20try_into%20for%20u64%20say%20infallible%20error.md)

---

There is no conversion`From<f64>` for `u64`. This question is marked `False` as a result of a misinterpretation of an error message produced by

````rust
let a: f64 = 1.5f64;
let b: Result<u64, _> = a.try_into()?;
````

due to the `?`.

For example,

````
error[E0277]: the trait bound `Result<u64, _>: TryFrom<f64>` is not satisfied
  --> src/silverbullet_syscalls/util.rs:40:31
   |
40 |     let b: Result<u64, _> = a.try_into()?;
   |                               ^^^^^^^^ the trait `From<f64>` is not implemented for `Result<u64, _>`
   |
   = help: the trait `From<PromiseState<T>>` is implemented for `Result<T, wasm_bindgen::JsValue>`
   = note: required for `f64` to implement `Into<Result<u64, _>>`
   = note: required for `Result<u64, http://localhost:3000/lan/2026/topic/000%20Atomic/wiki/002%20Wiki%20Questions/task/000%20False%20Why%20does%20rust%20f64%20try_into%20for%20u64%20say%20infallible%20error_>` to implement `TryFrom<f64>`
   = note: required for `f64` to implement `TryInto<Result<u64, _>>`
````

and above it in vscode, it says:

````
this can't be annotated with `?` because it has type `Result<_, Infallible>`rustc[E0277](https://doc.rust-lang.org/error-index.html#E0277)

util.rs(40, 41): original diagnostic
````

This can help with converting from `f64` to `u64`:

https://docs.rs/sample/latest/src/sample/conv.rs.html#557-571

From [rust reference Floating-point types](https://doc.rust-lang.org/stable/reference/types/numeric.html#r-type.numeric.float),

 > 
 > The IEEE 754-2008 “binary32” and “binary64” floating-point types are `f32` and `f64`, respectively.

It is a binary format, `b=2`. `p` is specified as 53 in (ref2).

From (ref2),

Some important values for binary64,

|key|value|
|---|-----|
|k|64|
|p|53|
|b|2|
|emax|1023|
|emin|1 - emax|
|w|11|
|t|52|

The bit encoding of a floating point number has 1 sign bit, `w` bits for the biased exponent, and `t` bits for the significand.

* For binary64
  * The largest floating point number is $b^\text{emax} \times (b - b^{1 - p})$ and the smallest is $b^\text{emin}$.
  * It is a binary format, `b=2`.
  * `p` is specified as 53.
  * `emax` is 1023
  * `emin` is 1 - emax

So the lowest possible value is

````python3
2 ** (1 - 1023)
````

````
2.2250738585072014e-308
````

With reciprocal \`4.49423283715579e+307\``

# References

1. https://doc.rust-lang.org/stable/reference/
   * Rust language reference

(ref2)

https://ieeexplore.ieee.org/document/4610935

754-2008 - IEEE Standard for Floating-Point Arithmetic

````
"IEEE Standard for Floating-Point Arithmetic," in _IEEE Std 754-2008_ , vol., no., pp.1-70, 29 Aug. 2008, doi: 10.1109/IEEESTD.2008.4610935.  
keywords: {IEEE Standards;Floating-point arithmetic;Microprocessors;Software;Hardware;Trademarks;754-2008;arithmetic;binary;computer;decimal;exponent;floating-point;format;interchange;NaN;number;rounding;significand;subnormal},
````
