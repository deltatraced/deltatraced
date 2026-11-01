---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[002 Setup openGL rendering app in rust and do some dev]]'
context_type: task
status: todo
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [002 Setup openGL rendering app in rust and do some dev](002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md)

Spawned in: [^spawn-task-1eea12](002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md#spawn-task-1eea12)

# 1 Journal

2026-05-21 Wk 21 Thu - 02:49 +03:00

https://docs.fileformat.com/font/ttf/

From https://www.wikihow.com/Install-TrueType-Fonts-on-Ubuntu,

`fc-cache -vr` (build font cache files), `fontconfig`, `gnome-font-viewer`, `font-manager`, `apt search ttf`, `fc-match <fontname>`

From https://askubuntu.com/a/384560,

* put `*.ttf` font in ~/.fonts then run `fc-cache -fv`

There is also fonts in `/usr/share/fonts/truetype/` for me.

````sh
lsb_release -a

# out
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 25.04
Release:        25.04
Codename:       plucky
````

2026-05-21 Wk 21 Thu - 05:09 +03:00

[gh harfbuzz/ttf-parser](https://github.com/harfbuzz/ttf-parser)

[fontspace.com open-source](https://www.fontspace.com/category/Open-source),

Let's install `Asana-Math`.

Installing via gui had it routed to `/home/lan/.local/share/fonts/Asana-Math.ttf`.

But it can also be put in `~/.font` and then installed with `fc-cache -fv` like the above instructions.

````sh
# in /home/lan/src/cloned/gh/deltachives/labeled-cube-rendering-2026-m000/rs
cargo add ttf_parser
````

2026-05-21 Wk 21 Thu - 07:22 +03:00

Found in `"lan-setup-notes/lan/topics/tooling/web/tasks/2025/000 Grep all comments on videos I have liked on youtube.md"`, for visidata enlarge text:

z+Enter v

From [reddit keyvalue order](https://www.reddit.com/r/learnrust/comments/1di2eb2/why_rust_prints_map_keyvalue_pairs_in_different/), using `BTreeMap` for quick rendering to json files to view vsdata in the key order I want.

2026-05-21 Wk 21 Thu - 08:10 +03:00

[docs.rs ttf_parser](https://docs.rs/ttf-parser/latest/ttf_parser/index.html)

2026-05-21 Wk 21 Thu - 08:29 +03:00

We can use this to get the glyph id for a given character.

````rust
let face = ttf_parser::Face::parse(&font_data, 0)?;
face.glyph_index('A')
````

2026-05-21 Wk 21 Thu - 09:45 +03:00

````rust
struct Builder(String);

// as per `face.outline_glyph` example
impl ttf_parser::OutlineBuilder for Builder {
    fn move_to(&mut self, x: f32, y: f32) {
        write!(&mut self.0, "M {} {} ", x, y).unwrap();
    }

    fn line_to(&mut self, x: f32, y: f32) {
        write!(&mut self.0, "L {} {} ", x, y).unwrap();
    }

    fn quad_to(&mut self, x1: f32, y1: f32, x: f32, y: f32) {
        write!(&mut self.0, "Q {} {} {} {} ", x1, y1, x, y).unwrap();
    }

    fn curve_to(&mut self, x1: f32, y1: f32, x2: f32, y2: f32, x: f32, y: f32) {
        write!(&mut self.0, "C {} {} {} {} {} {} ", x1, y1, x2, y2, x, y).unwrap();
    }

    fn close(&mut self) {
        write!(&mut self.0, "Z ").unwrap();
    }
}

fn example_render_image_of_char(face: &ttf_parser::Face) -> LcrResult<()> {
    let c = 'A';
    let gid = face.glyph_index(c)
        .ok_or(LcrError::GenericScriptError { s: "failed to find glyph index".to_owned() })?;

    // not available for 'A':
    // let raster_img = face.glyph_raster_image(gid, 1)
    //     .ok_or(LcrError::GenericScriptError { s: "Failed to create raster glyph image".to_owned() })?;
    // let svg_doc = face.glyph_svg_image(gid)
    //     .ok_or(LcrError::GenericScriptError { s: "failed to create svg doc".to_owned() })?;

    let mut builder = Builder(String::new());
    let bbox = face.outline_glyph(gid, &mut builder)
        .ok_or(LcrError::GenericScriptError { s: "failed to outline glyph".to_owned() })?;
    println!("builder.0: {}", builder.0);
    println!("bbox: {bbox:?}");

    // write_file_content(&PathBuf::from_str("doc.ign.svg")?, svg_doc.data, true)?;

    Ok(())
}
````

````
font_path: /home/lan/.fonts/Asana-Math.ttf
builder.0: M 408 700 L 650 132 Q 676 71 689.5 52 Q 703 33 722 30 L 756 27 L 756 -3 Q 702 -1 678.5 -0.5 Q 655 0 647 0 Q 639 0 629 0 Q 621 0 612.5 0 Q 604 0 578 -0.5 Q 552 -1 490 -3 L 490 27 L 537 30 Q 578 33 578 50 Q 578 57 574 70 Q 570 83 557 114 L 511 229 L 223 229 L 160 56 Q 160 34 208 30 L 245 27 L 245 -3 Q 191 -1 168 -0.5 Q 145 0 138 0 Q 131 0 124 0 Q 116 0 109 0 Q 102 0 82 -0.5 Q 62 -1 15 -3 L 15 27 L 52 30 Q 87 33 106 79 L 376 700 L 408 700 Z M 240 269 L 493 269 L 367 567 L 240 269 Z
bbox: Rect { x_min: 15, y_min: -3, x_max: 756, y_max: 700 }
````

So we're able to draw a given letter. Now we need to turn this information into a visual format.

* [stackoveflow post](https://stackoverflow.com/questions/71964574/fonttools-how-to-convert-glyphcoordinates-object-into-a-list-of-linear-and-quad)
  * $\to$ [jdhao.github.io post on bezier curves](https://jdhao.github.io/2018/11/27/font_shape_mathematics_bezier_curves/)
    * $\to$ [microsoft opentype spec](https://learn.microsoft.com/en-us/typography/opentype/spec/ttch01)
