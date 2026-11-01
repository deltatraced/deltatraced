---
parent: '[[000 Proc Wiki Labeled Cube Rendering]]'
spawned_by: '[[000 Proc Wiki Labeled Cube Rendering]]'
context_type: entry
---

Parent: [000 Proc Wiki Labeled Cube Rendering](../000%20Proc%20Wiki%20Labeled%20Cube%20Rendering.md)

Spawned by: [000 Proc Wiki Labeled Cube Rendering](../000%20Proc%20Wiki%20Labeled%20Cube%20Rendering.md)

Spawned in: [^spawn-entry-2b6fff](../000%20Proc%20Wiki%20Labeled%20Cube%20Rendering.md#spawn-entry-2b6fff)

# 1 Purpose

As we study the source and experiments, explanatory gaps and questions naturally arise. We capture them here. They may inspire an investigation to produce an explanation, and the explanation might end up in the wiki as a document as well.

# 2 Open Questions

# 3 Pend Questions

## 3.1 Q1

2026-05-13 Wk 20 Wed - 19:14 +03:00

* [ ] (1)

(Q)
We encountered this code while reproducing [\_2_2_hello_triangle_indexed.rs](https://github.com/bwasty/learn-opengl-rs/blob/master/src/_1_getting_started/_2_2_hello_triangle_indexed.rs),

````rust
gl::DrawElements(shader_prog_data.element_type, 6, gl::UNSIGNED_INT, ptr::null());
````

What does the `6` mean?
(/Q)

Observations:

* 2026-05-13 Wk 20 Wed - 19:35 +03:00
  * (1a) Set it to `< 3`, and no triangles render.
  * (1b) Set it to `>= 3`, and at least one triangle renders.
  * (1c) Set it to `>= 6`, and both triangles render.

Hypothesis:

1. 2026-05-13 Wk 20 Wed - 19:35 +03:00
   * Each triangle needs 3 points. Although the number of *unique* points is 4, the number of triangles times the number of points required per triangle is 6.
     * Based on Observations 1a-c.
