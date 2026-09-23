---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[002 Setup openGL rendering app in rust and do some dev]]'
context_type: task
status: done
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [002 Setup openGL rendering app in rust and do some dev](002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md)

Spawned in: [^spawn-task-16a003](002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md#spawn-task-16a003)

# 1 Journal

2026-05-18 Wk 21 Mon - 19:25 +03:00

Sketch for transforming any shape vertices to one cell in a 2x2, without taking into consideration margins:

![Pasted image 20260518193435.png](../../../../../../../attachments/Pasted%20image%2020260518193435.png)

2026-05-18 Wk 21 Mon - 23:27 +03:00

Since we use `InternalError` to encode invariants, it can often happen that the computations that shouldn't fail are happening in a module that you are currently consuming, and because of that, its `InternalError` might have to be added to your current module. But then this prevents you from being able to use `?` to not clutter the code with those cases possibly failing. With `thiserror`, you get `?` only up to the variants you directly support with `$[from]`, but not the variants of the variants, like the `InternalError`s of your consumed module.

In communications I ran into [docs.rs error_set](https://docs.rs/error_set/latest/error_set/) that could help solve this problem by erasing the nesting structure. As they mention:

 > 
 > With `error_set` there is no need to maintain a web of nested wrapped enums (with `#[from]`), since there is no nesting, and all the `From` implementations are automatically generated if one error type is a subset of another.

`cargo add error_set`

2026-05-20 Wk 21 Wed - 02:14 +03:00

Spawn [000 Looking into error crates and simplifying use of ? for errors between modules](../investigation/000%20Looking%20into%20error%20crates%20and%20simplifying%20use%20of%20%3F%20for%20errors%20between%20modules.md) ^spawn-invst-d25645

2026-05-20 Wk 21 Wed - 02:15 +03:00

Wrote functions and isomorphism test to prove that two encodings are identical, the index on the grid and an n-array of (left/right, down/up) choices where n is the degree, so 3 for 8x8 since $2^{3} = 8$.

````
	0  1  2  3  4  5  6  7
	-----------------------
7 | 56 57 58 59 60 61 62 63
6 | 48 49 50 51 52 53 54 55
5 | 40 41 42 43 44 45 46 47
4 | 32 33 34 35 36 37 38 39
3 | 24 25 26 27 28 29 30 31
2 | 16 17 18 19 20 21 22 23
1 | 8  9  10 11 12 13 14 15
0 | 0  1  2  3  4  5  6  7
````

It was also interesting to find that the critical points of this grid can be used directly to obtain the index from the choices. For example, 17 is in \`\[botleft, topleft, botright\]

And the critical points would be correspondingly `[0, 16, 1]`, whose sum is 17!

Each step in the array we need to consider the next botleft quadrant. The last one only has `0 1 8 9`, and its botright critical point is 1.

2026-05-20 Wk 21 Wed - 03:58 +03:00

Can be confusing if I'm not consistent with the const generic usize variables.

They should all be consistent with $n^m = r$ when working with the grid. We shouldn't just use `n` because we only need one generic, when really we mean `m` the power. And this means that `y_step`, which is `8` in the above `8x8` is just `r`, since that grid corresponds to $2^3 = 8$ .

We also used `m` prior for the number of times we recursed into a quadrant, but this should be something else, like `s`.

2026-05-20 Wk 21 Wed - 04:13 +03:00

Finally tried to run the grid rendering algorithm I made, but I got

````
thread 'main' panicked at /home/lan/src/cloned/gh/deltachives/labeled-cube-rendering-2026-m000/rs/src/lib.rs:558:9:
attempt to subtract with overflow
````

pointing to

````rust
// We need to consider the next botleft subgrid, so subtract by the critical point
// to shift the determined subgrid there.
let critical_points =
	calc_pow2_cell_critical_indices::<m>(InclusiveFinUsize32NZ::create(s)?);
let critical_point = critical_points[(leftright, updown)];
mut_index -= critical_point;
````

It shouldn't be possible for the critical point to be more than the index. It is always the lowest value in the given quadrant.

Oh `cargo test` fails now, so a refactor must have changed behaviors when I did the renaming of generics:

````
test tests::grid_index_to_turns_for_pow2_cell_tests::test_to_fro_1 ... FAILED
test tests::grid_index_to_turns_for_pow2_cell_tests::test_case1 ... FAILED
````

Yeah it was this:

````diff
-let scale = 2usize.pow(m.try_into().expect("unreachable"));
+let scale = 2usize.pow(s.try_into().expect("unreachable"));
````

Accidentally stayed as `m`, when refactoring the then-m to `s`. This should be increasing each iteration, while `m` stays constant now.

It was suspicious since it was identical to

````rust
let r = 2usize.pow(m.try_into().expect("unreachable"));
````

Which would be redundant.

2026-05-20 Wk 21 Wed - 04:40 +03:00

No more crashing, but still unexpected results for

````rust
let length: f32 = 2.00;

let LineConnectedShape { vertices, indices } =
	render_line_connected_shape_on_grid::<2>(
		&PowNSquareGridVec::create(vec![
			line_connect_lattice_filtered_circle(length, U32NZ::create(resolution)?)?,
			line_connect_lattice_filtered_circle(length, U32NZ::create(resolution)?)?,
			line_connect_lattice_filtered_circle(length, U32NZ::create(resolution)?)?,
			line_connect_lattice_filtered_circle(length, U32NZ::create(resolution)?)?,
		])?, IntervalF32::create(0.0)?)?;
````

We just basically see two small lines `\` and `/` at botleft and botright corners.

````rust
// in fn render_line_connected_shape_on_grid
let (translate_x, translate_y) = calc_center_translation_for_2pow_cell(&turns);
let slope = translate_x.abs();
````

This slope is wrong. The slope should be the lowest step amount possible given the grid, which would be for a $2^m$ grid $\frac{1}{2^{m+1}}$ where `m` is how many quadrants we can recurse into, ~~and it is `m+1` because at the final single cell, we still need to express its corners from its centers, so we are moving half the spread of the full lattice of the grid.~~

There was an off by one error in `calc_center_translation_for_2pow_cell` which we should test. But fixing this, we at least do see the circles now:

![Pasted image 20260520050208.png](../../../../../../../attachments/Pasted%20image%2020260520050208.png)

Although we're expecting a 2x2 grid, not this.

 > 
 > ~~and it is `m+1` because at the final single cell, we still need to express its corners from its centers, so we are moving half the spread of the full lattice of the grid.~~

Well, in a $2^m \times 2^m$ grid, expressing it already takes less than the total length/width, so this doesn't seem right. A single cell should be shrunk to $\frac{1}{2^{m}}$, so for a 4x4, that would be $\frac{1}{4}$ the size. And the translation should already be taking into account the finer translations each quadrant recursion step.

We have that

$$
C_n \equiv \left[ \begin{array}{c} \text{translate}_x + \frac{1}{2} x \\ \text{translate}_y + \frac{1}{2}y \end{array} \right]
$$

For mapping to some quadrant `Cn` from an image to a `2x2` grid. Although the quadrant is one fourth the area, the lengths themselves only halved once. The `2x2` is the `m=1` case, in general the lengths should shrink to $\frac{1}{2^m}$.

For some reason the circle is not reaching the edge of the screen, so for testing we can use

````rust
render_line_connected_shape_on_grid::<2>(
	&PowNSquareGridVec::create(vec![
		combine_line_connected_shape_layers_additively(
			&[
				line_connect_rat_circle(length, U32NZ::create(resolution)?)?,
				line_connect_line_segment(length, U32NZ::create(resolution)?, Orientation::I)?,
				line_connect_line_segment(length, U32NZ::create(resolution)?, Orientation::J)?,
			]
		)?,
		// ...
	])?, IntervalF32::create(0.0)?)?;
````

So it was working alright, but I was using `m=2`, so a `4x4`, and yet only displaying 4 entries since I thought I was doing a `2x2`. This is with the above combined shape repeating 16 times for a `4x4`:

![Pasted image 20260520055946.png](../../../../../../../attachments/Pasted%20image%2020260520055946.png)

and with some other shapes:

![Pasted image 20260520060506.png](../../../../../../../attachments/Pasted%20image%2020260520060506.png)

2026-05-20 Wk 21 Wed - 06:33 +03:00

````sh
# in /home/lan/src/cloned/gh/deltachives/labeled-cube-rendering-2026-m000/
git commit -m "impl 2pow grid rendering"
[main 7d5a6bc] impl 2pow grid rendering
 4 files changed, 309 insertions(+), 86 deletions(-)
````

2026-05-20 Wk 21 Wed - 08:44 +03:00

The encoding we had for the grid indices using successive quadrant selections is apparently similar to a data structure called a quadtree.

We could've also used a translated lattice by the spread amount to get a map from the 2D index to the translation center, and that would have also generalized to any square grid, not just $2^m \times 2^m$.

We also added margins. It's just an additional multiplier besides scaling the shape to its cell.

If we wanted, we could draw a translated grid to outline the cells, but not currently necessary.
