---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[000 Implement the Event Accumulator]]'
context_type: task
status: todo
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned in: [^spawn-task-b96c13](../000%20Implement%20the%20Event%20Accumulator.md#spawn-task-b96c13)

# 1 Journal

2025-09-20 Wk 38 Sat - 20:47 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
cargo add chrono

# out (relevant)
 Features:
 + alloc
 + clock
 + iana-time-zone
 + js-sys
 + now
 + oldtime
 + std
 + wasm-bindgen
 + wasmbind
 + winapi
 + windows-link
 - __internal_bench
 - arbitrary
 - core-error
 - libc
 - pure-rust-locales
 - rkyv
 - rkyv-16
 - rkyv-32
 - rkyv-64
 - rkyv-validation
 - serde
 - unstable-locales
````

We can use this to add a current time string for `created_on` field for events with

````rust
format!("{:?}", chrono::offset::Local::now())
````

2025-09-20 Wk 38 Sat - 21:04 +03:00

We also need some random numbers for cheap allocation of object ids

````sh
# in /home/lan/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs
cargo add rand

# out (relevant)
 Features:
 + alloc
 + os_rng
 + small_rng
 + std
 + std_rng
 + thread_rng
 - log
 - nightly
 - serde
 - simd_support
 - unbiased
````

2025-09-20 Wk 38 Sat - 21:32 +03:00

Actually, persons should be unique. To enforce the variant, so should be the object id. We might also want inserting events to be blocking (optionally) in which case we wait
