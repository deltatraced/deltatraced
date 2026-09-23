---
parent: '[[000 Learning]]'
spawned_by: '[[000 Learning]]'
context_type: entry
---

Parent: [000 Learning](../000%20Learning.md)

Spawned by: [000 Learning](../000%20Learning.md)

Spawned in: [^spawn-entry-0d3d79](../000%20Learning.md#spawn-entry-0d3d79)

[Mn 09 September](../../../../../2026-05-21_2025/main/overview/monthly/2025/Mn%2009%20September.md)

# 1 Purpose

Capturing highlights of practices and lessons learned this month!

# 2 Journal

(1)

2025-09-05 Wk 36 Fri - 13:48

Spawn [001 Streamlined environment variables in Rust](001%20Streamlined%20environment%20variables%20in%20Rust.md) ^spawn-entry-07b270

(2)

2025-09-19 Wk 38 Fri - 09:56 +03:00

From [001 Getting many debugging logs from rustyline while using shi > ^learning-6c9d3b](../../../../../2026-05-21_2025/microproj/001%20Rust%20Diesel%20Event%20Sourcing/tasks/2025/002%20Move%20credit_store_demo%20project%20to%20deltachives/issues/001%20Getting%20many%20debugging%20logs%20from%20rustyline%20while%20using%20shi.md#learning-6c9d3b),

We actually can do

````rust
#[error("Got ShiError: {0:?}")]
ShiError(#[from] ShiError),
````

And then we can do `?` on a `ShiError` directly! No need to map in the case it's a one to one correspondence.
