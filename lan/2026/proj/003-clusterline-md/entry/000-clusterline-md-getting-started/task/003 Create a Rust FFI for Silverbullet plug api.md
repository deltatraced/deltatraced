---
context_type: task
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/001 create a rust silverbullet plugin](001%20create%20a%20rust%20silverbullet%20plugin.md)

Spawned in: [^spawn-task-f6aec1](001%20create%20a%20rust%20silverbullet%20plugin.md#spawn-task-f6aec1)

# Journal

2026-05-25 Wk 22 Mon - 04:40 +03:00

Now we want to import the functions in as async:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello-wasm
cargo add wasm_bindgen_futures
````

We also want to use enums instead of strings, but can generate the needed string conversions automatically with [gh Peternator7/strum](https://github.com/Peternator7/strum):

````sh
# in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello-wasm
cargo add strum --features strum_macros
cargo add strum_macros
````

2026-05-25 Wk 22 Mon - 05:28 +03:00

We need to duplicate a lot of documentation as we create a rust clone of the plugin API. We included the license for silverbullet as a result.

We have to also recreate the structs, and maybe make them more type-contract flavored to reflect invariants about them. We’re going to use a unified `error_set::error_set!`:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello-wasm
cargo add error_set
````

We need to be able to read properties from `JsValue` to convert it to our types. We are going to be using `error_set` here.

2026-05-28 Wk 22 Thu - 03:34 +03:00

The API gives us the cursor position in `# of characters` via `editor.getCursor()`. But what’s the character encoding?

Previously we looked into f64 here:

* From [000 False Why does rust f64 try_into for u64 say infallible error](../../../../../topic/concept/000%20Atomic/wiki/002%20Wiki%20Questions/task/000%20False%20Why%20does%20rust%20f64%20try_into%20for%20u64%20say%20infallible%20error.md),
  * https://doc.rust-lang.org/stable/reference/

For rust, the character is:

* https://doc.rust-lang.org/stable/reference/types/char.html

From https://doc.rust-lang.org/stable/std/primitive.char.html,

 > 
 > * The `char` type represents a single character. More specifically, since ‘character’ isn’t a well-defined concept in Unicode, `char` is a ‘[Unicode scalar value](https://www.unicode.org/glossary/#unicode_scalar_value)’.
 > * Unicode scalar values are also the exact set of values that may be encoded in UTF-8.

SilverBullet uses `codemirror` for this.

https://codemirror.net/docs/ref/#state.EditorSelection

In the Text section,

 > 
 > * Line numbers start at 1. Character positions are counted from zero, and count each line break and UTF-16 code unit as one unit.

Since they are counts of units, not counts of bytes, we should expect that they continue to hold as we do a unit conversion to get a Rust string.

2026-05-28 Wk 22 Thu - 08:04 +03:00

Some vim-based automation as we port these APIs. Some are simple to convert, taking no arguments, and giving nothing.

````sh
# Convert for functions that take no arguments and return nothing

%s/export function /pub async fn /g
%s/: Promise<void> {/;/g
%g/return/d
%g/}/d

# Every function should get a line `#[wasm_bindgen(js_namespace ="editor")]`
# before it in the imported mod.
# Options from "000 Document event flags for textscript.md" in dism-exe-notes

sed -zi "s/\(\n *pub[^\n]*\)/\n\t\t\t\t#[wasm_bindgen(js_namespace = \"editor\")]\1/g" b
````

2026-05-28 Wk 22 Thu - 09:44 +03:00

You can have vim live update a file with

````vim
:set autoread | au CursorHold * checktime | call feedkeys("lh")
````

from https://stackoverflow.com/a/48296697/6944447

More automated, `a` has the original content:

````sh
# set autoread | au CursorHold * checktime | call feedkeys("lh")

# Convert for functions with normal parameters and returns

cp a b

sed -i "s/export async function /beep async fn /g" b
sed -i "s/export function /pub async fn /g" b
sed -i "s/LuaCollectionQuery/JsValue/g" b
sed -i "s/<T>//g" b
sed -i "s/: Promise<void> {/;/g" b
sed -i "s/: Promise<Uint8Array> {/ -> Vec<u8>;/g" b
sed -i "s/: Promise<boolean> {/ -> bool;/g" b
sed -i "s/: Promise<number> {/ -> f64;/g" b
sed -i "s/: Promise<.*\[\]> {/ -> Vec<JsValue>;/g" b
sed -i "s/: Promise<.* | null> {/ -> Option<JsValue>;/g" b
sed -i "s/: Promise<.* | undefined> {/ -> Option<JsValue>;/g" b
sed -i "s/: Promise<.*> {/ -> JsValue;/g" b
sed -i "s/\((\|, \)\([a-zA-Z0-9_]*\)?: string/\1opt_\2: Option<String>/g" b
sed -i "s/\(\w*\)?: number/opt_\1: Option<f64>/g" b
sed -i "s/\(\w*\)?: string\[\]/opt_\1: Option<Vec<String>>/g" b
sed -i "s/\(\w*\)?: .*/opt_\1: Option<JsValue>/g" b
sed -i "s/: string\[\]/: Vec<String>/g" b
sed -i "s/: any\[\]/: Vec<JsValue>/g" b
sed -i "s/: [a-zA-Z0-9_]*\[\]/: Vec<JsValue>/g" b
sed -i "s/: string/: \&str/g" b
sed -i "s/: any/: JsValue/g" b
sed -i "s/: Uint8Array/: \&[u8]/g" b
cat b | grep -v "return" > c; mv c b
cat b | grep -v "}" > c; mv c b

# Every function should get a line `#[wasm_bindgen(js_namespace ="MODULE")]`
# before it in the imported mod.
# Options from "000 Document event flags for textscript.md" in dism-exe-notes

cp a b
sed -zi "s/\(\n *pub[^\n]*\)/\n\t\t\t\t#[wasm_bindgen(js_namespace = \"system\")]\1/g" b

## Convert imported functions which are identical in parameters and return

cp a b

cat b | grep -v "#\[wasm_bindgen" > c; mv c b
sed -zi "s/pub async fn \([a-zA-Z0-9_]*\)(\([^\n)]*\))\([^\n]*\);/pub async fn \1(\2)\3{\nimported::\1@INPUT(\2@INPUT).await\n}/g" b
sed -i "s/@INPUT(\([a-zA-Z0-9_]*\):\([^,]*\)\([^)@]*\)@INPUT)/@INPUT(\1\3@INPUT)/g" b
sed -i "s/@INPUT(\([a-zA-Z0-9_]*\), \([a-zA-Z0-9_]*\):\([^,]*\)\([^)@]*\)@INPUT)/@INPUT(\1, \2\4@INPUT)/g" b
sed -i "s/@INPUT(\([a-zA-Z0-9_]*\), \([a-zA-Z0-9_]*\), \([a-zA-Z0-9_]*\):\([^,]*\)\([^)@]*\)@INPUT)/@INPUT(\1, \2, \3\5@INPUT)/g" b
sed -i "s/@INPUT//g" b

# Remove documentation

cp a b

cat b | grep -v "*" > c; mv c b
````

2026-05-29 Wk 22 Fri - 00:36 +03:00

`&s[n..m]` is byte-range and thus unsafe to use generally unless you have an invariant that your string is guaranteed to be ASCII. See https://doc.rust-lang.org/stable/std/slice/trait.SliceIndex.html#associatedtype.Output-1

2026-05-29 Wk 22 Fri - 07:12 +03:00

We need to understand what’s `KvKey`. It’s a type equivalent to `string[]`  and yet `batchSet` in `silverbulletmd/silverbullet/client/data/datastore.ts` just stringifies it!

Then there’s the test `runDataStoreTest` in `silverbulletmd/silverbullet/client/data/datastore.test.ts` which sets a dictionary with a `name` prop to the key `[“user”, “peter”]`, but then it does a lua query to it, and even though it’s a JSON, space lua is able to pick this apart, so that we have it set to just `“user”`.

2026-05-29 Wk 22 Fri - 07:51 +03:00

Keeping `LuaExpression` as an opaque `JsValue` for now. It exposes the entire grammar of space lua, which might be too unstable a detail to expose (also there’s a lot, so maybe not now or depending on the demand).

Note this does not mean we can’t work with lua expressions. The `lua` api lets us evaluate expressions.

2026-05-29 Wk 22 Fri - 11:36 +03:00

We need to create our own `JsValue` objects where serde cannot infer.

https://users.rust-lang.org/t/wasm-bindgen-how-do-i-manually-create-and-manipulate-a-js-object/26653/3

2026-05-30 Wk 23 Sat - 08:37 +03:00

Using `cargo expand | less` and searching `CategoryDefinition` we are able to see how `#[derive(Serialize)]` expands for it. We will have mismatch of field names because we use names like `opt_description`, while on typescript it would be `description`:

````rust
                        _serde::ser::SerializeStruct::serialize_field(
                            &mut __serde_state,
                            "opt_description",
                            &self.opt_description,
                        )?;
````

So we do not have one-to-one correspondence outside the `imported` module and we shouldn’t assume, we need to revert these auto-serialized structs and manually convert them to `JsValue`. In fact, let’s remove

````
serde = { version = "1.0", features = ["derive"] }
serde-wasm-bindgen = "0.4"
````

These dependencies are unnecessary, and we want to lower our library footprint where reasonable for the template and api crates. serde is doing a lot of heavy lifting with data structures.

2026-05-30 Wk 23 Sat - 09:18 +03:00

Also removing `opt_to_js_value` because this is duplicate. Searching for the implementation of `From<Option<T>>` for `JsValue` shows that. It’s in `impl<T> From<Option<T>> for JsValue`, `/home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/wasm-bindgen-0.2.122/src/lib.rs`.

We used `js_sys::Undefined::UNDEFINED.into()` wheras they use `JsValue::undefined()`.

2026-05-30 Wk 23 Sat - 14:02 +03:00

We need `Blob`:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello-wasm
cargo add web_sys --features Blob

````

2026-05-30 Wk 23 Sat - 23:31 +03:00

https://stackoverflow.com/a/38317664/6944447 $\to$ https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types

Need to handle these `&` types correctly, since they join the members of both sides

Let’s not support the case where a member appears in both the left and right components, as we would need then to ensure they are of the same type, and ensuring they are of the same value later on.

2026-05-31 Wk 23 Sun - 04:28 +03:00

We need to also handle tuple types: https://www.typescriptlang.org/docs/handbook/2/objects.html#tuple-types

They seem to behave exactly like arrays of JsValue, so let’s just turn them into that.

2026-05-31 Wk 23 Sun - 12:05 +03:00

Spawn [000 InternalErrors must not be exposed to module consumers](../judgment/000%20InternalErrors%20must%20not%20be%20exposed%20to%20module%20consumers.md) ^spawn-jdgmt-3ba711

2026-06-01 Wk 23 Mon - 08:20 +03:00

````rust
pub struct ParseTree<'a> {
    pub opt_type: Option<String>,
    pub opt_from: Option<u64>,
    pub opt_to: Option<u64>,
    pub opt_text: Option<String>,
    pub opt_children: Option<Vec<ParseTree<'a>>>,
    pub opt_parent: Option<&'a ParseTree<'a>>,
}
````

We can either have N immutable borrows, or 1 mutable borrow simultaneously. But to construct the above data type, we would need to be able to change the parent to include the children, and let the children borrow reference to the parent simultaneously, so we cannot do it.

We’re going to use an index-based solution instead, so that the references are opaque, like File IDs.

2026-06-01 Wk 23 Mon - 15:03 +03:00

Going the other way is also a challenge. How could we create a parent JsValue that refers to a child JsValue that refers to the parent JsValue?

If we assign one JsValue to a property in another, is this a copy by value or a copy by reference?

2026-06-02 Wk 23 Tue - 05:35 +03:00

````rust
// in wasm-bindgen-0.2.122/src/lib.rs

/// Representation of an object owned by JS.
///
/// A `JsValue` doesn't actually live in Rust right now but actually in a table
/// owned by the `wasm-bindgen` generated JS glue code. Eventually the ownership
/// will transfer into Wasm directly and this will likely become more efficient,
/// but for now it may be slightly slow.
pub struct JsValue {
    idx: u32,
    _marker: PhantomData<*mut u8>, // not at all threadsafe
}
````

So whether the `JsValue` is owned or not, when we create one we’re really just working with a handle. We can just set those handles for the parents and children and it should work as expected.

2026-06-02 Wk 23 Tue - 08:41 +03:00

Will document things I don’t get in [000 Silverbullet API Documentation Confusion](../../../../../topic/oss/000%20OSS%20Contrib/wiki/000%20Wiki%20OSS%20Contribution/entry/000%20Silverbullet%20API%20Documentation%20Confusion.md)

Should remember to go over everything again later and add stuff there.

---

2026-06-03 Wk 23 Wed - 16:52 +03:00

Porting the API is preliminarily done, with some exclusions made like for the lua grammar and desktop-app-only fields for CommandDef.

2026-06-03 Wk 23 Wed - 17:01 +03:00

`println` is not mapped to `console.log`. We brought in `console.log`.

Panicing only gives a `RuntimeError: unreachable executed` in the webassembly module. We need to enable better panic messages.

We need to enable this:

````rust
pub fn set_panic_hook() {
    // When the `console_error_panic_hook` feature is enabled, we can call the
    // `set_panic_hook` function at least once during initialization, and then
    // we will get better error messages if our code ever panics.
    //
    // For more details see
    // https://github.com/rustwasm/console_error_panic_hook#readme
    #[cfg(feature = "console_error_panic_hook")]
    console_error_panic_hook::set_once();
}
````

We already have:

````toml
# The `console_error_panic_hook` crate provides better debugging of panics by
# logging them with `console.error`. This is great for development, but requires
# all the `std::fmt` and `std::panicking` infrastructure, so isn't great for
# code size when deploying.
console_error_panic_hook = { version = "0.1.7", optional = true }

````

So we just enable:

````toml
[features]
#default = ["console_error_panic_hook"]
````

Much better!

````
[hello plug] panicked at src/lib.rs:15:5:
How do we panic???
````

2026-06-03 Wk 23 Wed - 17:48 +03:00

So we get a panic when we try to write a file:

````rust
log(&format!("{:?}", write_file("rustplug", &vec![1, 2, 3, 4]).await));
````

````
[hello plug] panicked at src/silverbullet_plug_api/space.rs:16:5:
uncaught exception: JsValue(Error: Couldn't write file, path is not writable
S/</<@http://localhost:3000/.fs/Library/LanHikari22/clusterlinemd/hello.plug.js:1:2397
S/<@http://localhost:3000/.fs/Library/LanHikari22/clusterlinemd/hello.plug.js:1:2445
)
````

2026-06-04 Wk 23 Thu - 00:17 +03:00

Functions like `writeFile` seem to come from here:

````ts
// in /home/lan/src/cloned/gh/silverbulletmd/silverbullet/build/build_client.ts
import { cp, mkdir, readdir, readFile, writeFile } from "node:fs/promises";
````

https://nodejs.org/api/all.json

`modules` > `File system` > `Promises API` > `methods` > `fsPromises.writeFile`

https://nodejs.org/api/fs.html#filehandlewritefiledata-options

[gh nodejs/node WriteFileUtf8](https://github.com/nodejs/node/blob/af2e68bba5a81f795c9e351110145d24789e9361/src/node_file.cc#L2724)

2026-06-04 Wk 23 Thu - 01:29 +03:00

We just document for the user that panics are inherited from node.js.

We are able to do

````rust
log(&format!("{:?}", write_file("rustplug.md", &vec![1, 2, 3, 4]).await));
````

````
[hello plug] FileMeta { name: "rustplug.md", created: 1780525666703, last_modified: 1780525666704, content_type: "text/markdown", size: 4, perm: Rw }
````

This creates a `rustplug.md` in the space root. It can create a `rustplug.bin` too. Seems its issue was lack of extension.

2026-06-04 Wk 23 Thu - 05:28 +03:00

We could have also used the snake case conversion on the extern “C” wasm_bindgen functions, according to the docs on one of them like

````
hello_wasm::silverbullet_plug_api::editor::imported
pub async fn getText() -> String
hello_wasm::silverbullet_plug_api::editor::imported
unsafe fn __wbg_getText_6871b6f33b97eb96() -> wasm_bindgen::convert::WasmRet<<wasm_bindgen_futures::js_sys::Promise<String> as wasm_bindgen::convert::FromWasmAbi>::Abi>
````

````
A Note About camelCase, snake_case, and Naming Conventions
JavaScript's global objects use camelCase naming conventions for functions and methods, but Rust style is to use snake_case. These bindings expose the Rust style snake_case name. Additionally, acronyms within a method name are all lower case, where as in JavaScript they are all upper case. For example, decodeURI in JavaScript is exposed as decode_uri in these bindings.
````
