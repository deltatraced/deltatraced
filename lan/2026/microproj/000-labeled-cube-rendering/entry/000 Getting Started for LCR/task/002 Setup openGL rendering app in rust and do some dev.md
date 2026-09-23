---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[000 Getting Started for LCR]]'
context_type: task
status: todo
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned in: [^spawn-task-69c8c6](../000%20Getting%20Started%20for%20LCR.md#spawn-task-69c8c6)

# 1 Journal

2026-05-13 Wk 20 Wed - 00:35 +03:00

Copy the template rust project from `~/src/cloned/gh/deltachives/2025-003-tmpl-lan-rs/`:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/rs
mv 2025-003-tmpl-lan-rs/* .
mv 2025-003-tmpl-lan-rs/.gitignore .
mv 2025-003-tmpl-lan-rs/.pre-commit-config.yaml .
````

Modify copyright in README to 2026.

This could be useful: https://rust-tutorials.github.io/learn-opengl/

Let's try to use the same dependencies:

````toml
[dependencies]
bytemuck = "1"
ogl33 = { version = "0.2.0", features = ["debug_error_checks"]}

[dev-dependencies]
beryllium = "0.2.0-alpha.4"
imagine = "0.0.5"
````

2025-21-pre
Building `imagine` requires `SDL2` ([SDL](https://www.libsdl.org/)).

````sh
sudo apt-get install libsdl2-dev
````

`beryllium = "0.2.0-alpha.4"` asks for sdl 2.0, but I have `sdl2-config --version` as `2.32.2`.

[gh Lokathor/beryllium](https://github.com/Lokathor/beryllium). [gh Lokathor/fermium](https://github.com/Lokathor/fermium).

They did mention static linking too but the issue persists:

````
beryllium = { version = "0.2.0-alpha.1", default-features = false, features = ["link_static"] }
````

2026-05-13 Wk 20 Wed - 02:58 +03:00

We can also try

[gh bwasty/learn-opengl-rs](https://github.com/bwasty/learn-opengl-rs)

They are using the depenencies:

````
cgmath = "0.16.1"
gl = "0.10.0"
glfw = "0.23.0"
image = "0.19.0"
# only needed from chapter 3 on
tobj = "0.1.6"
num = "0.2.0"
rand = "0.5.5"
````

````
warning: the following packages contain code that will be rejected by a future version of Rust: nom v1.2.4
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/rs
cargo run

# out
   Compiling lcr v0.1.0 (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/rs)
error: linking with `cc` failed: exit status: 1
  |
  = note:  "cc" "-m64" "/tmp/rustc4WQbiF/symbols.o" "<257 object files omitted>" "-Wl,--as-needed" "-Wl,-Bstatic" "/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/rs/target/debug/deps/{libgl-e95c6c41637ca913.rlib,libglfw-a73e8674f72a0b3b.rlib,libnum-966f4bbdfd2f537c.rlib,libnum_rational-1fa9594825134919.rlib,libnum_bigint-42e230b73e5d20db.rlib,librand-6f3fe1d80060e6fb.rlib,libnum_complex-370a997a45756c40.rlib,librustc_serialize-949e60a5b41ebf98.rlib,libnum_iter-57f5c18dde951cff.rlib,libnum_integer-c2b6d35c869747a2.rlib,libenum_primitive-5cdd8272c6680a91.rlib,libnum_traits-83c27ecfd906b666.rlib,libnum_traits-5efab631036be2bc.rlib,libbitflags-b4c3be1feed9090f.rlib,liblog-8010545d5df8ff7c.rlib,liblibc-0700ff5dc4482962.rlib,libsemver-c19c6bf17f0c889a.rlib,libnom-94df99bda5d8ed59.rlib}.rlib" "<sysroot>/lib/rustlib/x86_64-unknown-linux-gnu/lib/{libstd-*,libpanic_unwind-*,libobject-*,libmemchr-*,libaddr2line-*,libgimli-*,librustc_demangle-*,libstd_detect-*,libhashbrown-*,librustc_std_workspace_alloc-*,libminiz_oxide-*,libadler2-*,libunwind-*,libcfg_if-*,liblibc-*,librustc_std_workspace_core-*,liballoc-*,libcore-*,libcompiler_builtins-*}.rlib" "-Wl,-Bdynamic" "-lX11" "-lGL" "-lXxf86vm" "-lXrandr" "-lXi" "-lXcursor" "-lXinerama" "-lgcc_s" "-lutil" "-lrt" "-lpthread" "-lm" "-ldl" "-lc" "-L" "/tmp/rustc4WQbiF/raw-dylibs" "-Wl,--eh-frame-hdr" "-Wl,-z,noexecstack" "-L" "/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/rs/target/debug/build/glfw-sys-65a8e308304676e6/out/lib" "-L" "<sysroot>/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-o" "/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/rs/target/debug/deps/lcr-c02f8ee1f6c8cc51" "-Wl,--gc-sections" "-pie" "-Wl,-z,relro,-z,now" "-nodefaultlibs"
  = note: some arguments are omitted. use `--verbose` to show all linker arguments
  = note: /usr/bin/ld: cannot find -lXxf86vm: No such file or directory
          collect2: error: ld returned 1 exit status


error: could not compile `lcr` (bin "lcr") due to 1 previous error
````

[stackoverflow](https://stackoverflow.com/a/12187682/6944447)  $\to$ `sudo apt-get install libxxf86vm-d2025-21-pre2025-21-pre2025-21-preev`

We are able to open a window now according to [1_1_hello_window.rs](https://github.com/bwasty/learn-opengl-rs/blob/master/src/_1_getting_started/_1_1_hello_window.rs)

We should be able to also add an interactive shell on a different thread to control the rendering. We have an example in `~/src/cloned/gh/deltachives/2025-002-credit-store-demo-rs/src/bin/demo.rs`.

Bringing in `drivers/shell.rs`.

With some shared global state, we're now able to sync the shell exiting with breaking the OpenGL render loop.

2026-05-13 Wk 20 Wed - 19:51 +03:00

````rust
let vertices: [f32; 12] = [
	0.5, 0.5, 0.0, // top right
	0.5, -0.5, 0.0, // bottom right
	-0.5, -0.5, 0.0, // bottom left
	-0.5, 0.5, 0.0, // top left
];
````

We can generalize this as a 2D lattice with length 1, and spread 0.5, meaning that from its far reach left to right, it should be able to cover three points: `. -0.5-> . -0.5-> .`.

The resolution is also the length over the spread. In this case, it is 2.

Right now we create vertices only for

````
  0  1  2
0 .     .
1
2 .     .
````

But we can also just have the entire lattice, and then work with the indices of interest.

````
  0  1  2
0 .  .  .
1 .  .  .
2 .  .  .
````

````
  0  1  2
0 0     1
1
2 2     3

  0  1  2
0 0  1  2
1 3  4  5
2 6  7  8
````

2025-21-pre2025-21-pre
2026-05-13 Wk 20 Wed - 20:40 +03:00

Hmm. We ended up drawing a triangle

````
  0  1  2
0        
1    .   
2 .  .   

````

For indices `0, 1, 4`.

````rust
pub fn create_2d_lattice_vertices(length: u32, resolution: u32) -> Vec<f32> {
    let mut mut_out = vec![];

    let spread = length as f32 / resolution as f32;
    let max_xy = length as f32 / 2f32;
    let mut mut_x = - max_xy;
    let mut mut_y = - max_xy;

    while mut_y <= max_xy {
        while mut_x <= max_xy {
            mut_out.push(mut_x);
            mut_out.push(mut_y);
            mut_out.push(0f32);

            mut_x += spread;
        }
            mut_y += spread;
            mut_x = - max_xy;
    }

    mut_out
}
````

I assumed we had the axis like this:

````
.--> x
|
v
y
````

So that the least on both should correspond to the topleft corner.

It seems we instead have

````
  0  1  2
2 6  7  8
1 3  4  5
0 0  1  2
````

Drawing the following triangles `0` and `1` also match.
let r = 2usize.pow(m.try_into().expect("unreachable"));

````
  0  1  2
2 1     1
1 0     1
0 0     0
````

2026-05-13 Wk 20 Wed - 22:30 +03:00

Switching to `gl::LINE` instead of `gl::TRIANGLES` makes nothing render.

````rust
ShaderProgramData {
	shader_program,
	vao,
	element_type: gl::LINE,
	num_indices,
}

// ...

gl::DrawElements(
	shader_prog_data.element_type,
	shader_prog_data.num_indices as i32,
	gl::UNSIGNED_INT,
	ptr::null(),
);
````

2025-21-pre2025-21-preBut we're still able to draw lines with `gl::TRIANGLES`, just by setting two indices to be the same.

2026-05-14 Wk 20 Thu - 07:41 +03:00

With `get_connected_neighbors`, we're now able to apply a filter on the lattice and connect all 1-index apart points. This algorithm is O(n) with respect to the resolution, because we don't have to check every other point, we already know the neighbors for any index in O(1) by definition of the lattice, and so it is enough to iterate through each point in the lattice, and connect it to its neighbors, if they are kept by the filter.

2026-05-15 Wk 20 Fri - 04:14 +03:00

Spawn [001 Getting a segfault when using CreateShader again](../issue/001%20Getting%20a%20segfault%20when%20using%20CreateShader%20again.md) ^spawn-issue-423b4f

2026-05-15 Wk 20 Fri - 06:36 +03:00

So we can also switch the lattice solution for a solution that generates only the needed vertices of the shape directly. Then for the indices, we can simply connect them by lines in terms of the order they appear in.

We also want points on a line segment with configurable resolution and length to map to the shape we want, in this case the circle using [003 Archived Rational Parameterization of the circle](../../../../../../archived/2026-05-21_2026/topic/study/math/wiki/2026/001%20Math%20Problems/tasks/003%20Archived%20Rational%20Parameterization%20of%20the%20circle.md):

````haskell
e : (h : ℚ) → Vect2
e h .x = (1 - h²) / (1 + h²)
e h .y = 2h / (1 + h²)
````

2026-05-16 Wk 20 Sat - 20:02 +03:00

We also need a notion of layers. Some shapes we want to draw are multipart.

For example a square is just one horizontal and one vertical line segments, with 2 more copies that are translations alongside the length of each one.

There is also the directed line segment `--->` which has the line, and smaller parts making the arrow part.

Layers should also be mergable into a full object to be rendered. Since our rendering strategy right now uses lines via reduced triangles, we can compile layers of this to a full object. This should let us

2026-05-18 Wk 21 Mon - 16:04 +03:00

So increasing the resolution by 1, through defining the spread as length over resolution meant for example to add 1 more edge to the vertical line

````
.---.---. (resolution 2)
.---.---.---. (resolution 3)
````

With resolution 2, a spread is 0.5 the length, hence there will only be 2 edges.

2026-05-18 Wk 21 Mon - 16:08 +03:00

We can hypothesize that the logical length from the bottom to the top of the rendering area is `2` from this:

````rust
let LineConnectedShape { vertices, indices } =
	line_connect_vertical_line_segment(1.98, resolution).unwrap();
````

which visibly almost cuts the screen in half at the center, but not quite (0.02 still leaves some visible gap)

Updating that function to a more general form:

````rust
let LineConnectedShape { vertices, indices } =
	line_connect_line_segment(1.98, resolution, Orientation::J).unwrap();
````

Note also with length `1`, a vertical line is 0.5 above and below from the center.

We get similar results for horizontal.

2026-05-18 Wk 21 Mon - 17:06 +03:00

Writing `line_connect_terminals_cut_line_segment`,

Cutting both the starting and ending vertices (and also indices) results in an unevenly looking cut line from left to right, where left seems to be cut more.

````
.---.---.---.---.

````

It might also have to do with the indices now being out of sync after the vertices have been cut, we will end up having fewer larger values, and end up creating less reduced triangles to the right than to the left. Might be best to regenerate the indices for the new reduced vertices.

2026-05-20 Wk 21 Wed - 08:43 +03:002025-21-pre2025-21-pre2025-21-pre

Spawn [003 Render an array of shapes onto a grid](003%20Render%20an%20array%20of%20shapes%20onto%20a%20grid.md) ^spawn-task-16a003

2026-05-20 Wk 21 Wed - 09:05 +03:00

We can use the quadrant encoding for centers to get some logical points of reference for our cube. For example within a 4x4, we can draw the two squared like so:

````
C D_E_F
  |   |
8_9_A B
| | | |
4 5_6_7
|   |
0_1_2 3
````

Since we know the location of any point here via quadrant indexing (or just using a lattice), we can draw any line on the cube using the select two vertices of an interest, and a single triangle index `[0 0 1]`.

Lines on the cube can be uniquely identified by a directed ~~2 choose 3~~:

````rust
pub enum Directed2Choose3 {
    FFX,
    FTX,
    TFX,
    TTX,

    FXF,
    FXT,
    TXF,
    TXT,

    XFF,
    XFT,
    XTF,
    XTT,
}
````

`X` is the direction of extension, and the position `ijk` that isn't `X` identifies two faces sharing a line. Note that it is impossible to select two opposite faces here, because one entry must always be a don't-care or `X`.

More generally,

````rust
pub enum TwoChooseThree<T> {
    TTX(T, T),
    TXT(T, T),
    XTT(T, T),
}
````

It's a `TwoChooseThree<IntervalEnd>`.

Oops, I meant `ThreeChooseTwo<IntervalEnd>`.
2025-21-pre2025-21-pre
2026-05-20 Wk 21 Wed - 21:39 +03:00

````python
  for i in range(15, -1, -1):
      for j in range(0, 16):
          n = 16*i + j
          print(f'{n: 4} ', end='')
      print('')
````

to get the data for the 16x16 grid which we will try to use to draw a cube through its indices. We need enough room on the left and right to be able to have our angles for the directed arrows.

With some styling:

````
            0    1    2    3    4    5    6    7    8    9    10   11   12   13   14   15
            ------------------------------------------------------------------------------
       15 | 240  241  242  243  244  245  246  247  248  249  250  251  252  253  254  255
       14 | 224  225  226  227  228  229  230  231  232  233  234  235  236  237  238  239
       13 | 208  209  210  211  212  213  214  215  216  217  218  219  220  221  222  223
       12 | 192  193  194  195  196  197  198  199  200  201  202  203  204  205  206  207
       11 | 176  177  178  179  180  181  182  183  184  185  186  187  188  189  190  191
       10 | 160  161  162  163  164  165  166  167  168  169  170  171  172  173  174  175
       9  | 144  145  146  147  148  149  150  151  152  153  154  155  156  157  158  159
       8  | 128  129  130  131  132  133  134  135  136  137  138  139  140  141  142  143
       7  | 112  113  114  115  116  117  118  119  120  121  122  123  124  125  126  127
       6  |  96   97   98   99  100  101  102  103  104  105  106  107  108  109  110  111
       5  |  80   81   82   83   84   85   86   87   88   89   90   91   92   93   94   95
       4  |  64   65   66   67   68   69   70   71   72   73   74   75   76   77   78   79
       3  |  48   49   50   51   52   53   54   55   56   57   58   59   60   61   62   63
       2  |  32   33   34   35   36   37   38   39   40   41   42   43   44   45   46   47
       1  |  16   17   18   19   20   21   22   23   24   25   26   27   28   29   30   31
       0  |   0    1    2    3    4    5    6    7    8    9   10   11   12   13   14   15
````

2026-05-20 Wk 21 Wed - 22:51 +03:00

`cargo add eros`

It would be good to have context on where this occurs:

````
thread 'main' panicked at examples/expt000_hello_circle.rs:360:74:
called `Result::unwrap()` on an `Err` value: FinUsizeGridNPowErrorHigherThanMax { data: 17, n: 2, m: 2, max: 16 }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
````

Though seems we'd have to use a `.map_err` to make this work with `eros::Result`:

````rust
        let c_str_frag = CString::new(fragment_shader_source.as_bytes()).map_err(Into::<LcrError>::into).into_traced()?;
````

Oh the error is because of this `<2>`:

````rust
// in fn line_connect_directed_line_on_2d_projected_cube(
let turns1 = grid_index_to_turns_for_2pow_cell::<2>(FinUsizeGridNPow::create(index1)?)?;
````

We're working in a 16x16 now, so `m = 4`

2026-05-20 Wk 21 Wed - 23:23 +03:00

````rust
let LineConnectedShape { vertices, indices } =
	render_line_connected_shape_on_grid::<2>(
		&PowNSquareGridVec::create(vec![
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i0))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i0))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i1))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i1))?,

			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i0))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i0))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i1))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i1))?,

			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i0))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i0))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i1))?,
			line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i1))?,

			combine_line_connected_shape_layers_additively(
				&[
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i1))?,

					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i1))?,

					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i1))?,
				]
			)?,

			combine_line_connected_shape_layers_additively(
				&[
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i1))?,

					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i1))?,
				]
			)?,

			combine_line_connected_shape_layers_additively(
				&[
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i1))?,

					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::XTT(i1, i1))?,
				]
			)?,

			combine_line_connected_shape_layers_additively(
				&[
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TTX(i1, i1))?,

					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i0))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i0, i1))?,
					line_connect_directed_line_on_2d_projected_cube(ThreeChooseTwo::TXT(i1, i1))?,
				]
			)?,
		])?, IntervalF32::create(0.05)?)?;
````

![Pasted image 20260520231552.png](../../../../../../../attachments/Pasted%20image%2020260520231552.png)

Hmm. Something went wrong with the vertical line to the botright. with an angle `>`.

That line should be `TTX((i1, i1))`. Might also be good to half the vertex for the angles so they look a bit smaller.

![Pasted image 20260520234040.png](../../../../../../../attachments/Pasted%20image%2020260520234040.png)

Next to figure out annotated vertices and edges. It should be similar to the ascii art

![Pasted image 20260521000335.png](../../../../../../../attachments/Pasted%20image%2020260521000335.png)

![Pasted image 20260521011021.png](../../../../../../../attachments/Pasted%20image%2020260521011021.png)

Hmm the diagonal arrows don't look right. They should be directly left and down:

![Pasted image 20260521011708.png](../../../../../../../attachments/Pasted%20image%2020260521011708.png)

2026-05-21 Wk 21 Thu - 02:01 +03:00

Okay we need to figure out rendering text now.

Spawn [004 Installing and rendering fonts with GLFW and OpenGL](004%20Installing%20and%20rendering%20fonts%20with%20GLFW%20and%20OpenGL.md) ^spawn-task-1eea12
