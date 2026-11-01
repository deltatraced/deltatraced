---
context_type: issue
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/001 create a rust silverbullet plugin](../task/001%20create%20a%20rust%20silverbullet%20plugin.md)

Spawned in: [^spawn-issue-7ed0f8](../task/001%20create%20a%20rust%20silverbullet%20plugin.md#spawn-issue-7ed0f8)

# Journal

2026-06-04 Wk 23 Thu - 03:01 +03:00

This gets us the content of this page fine:

````ts
await editor.flashNotification("(0) It is you typescript...");
let s = await editor.getText();
console.log("text: " + s);
````

But this fails:

````rust
mod imported {
    use wasm_bindgen::prelude::*;

    #[wasm_bindgen(module = "@silverbulletmd/silverbullet/syscalls")]
    extern "C" {
        #[wasm_bindgen(js_namespace = "editor")]
        pub async fn getText() -> String;
    }
}

pub async fn get_text() -> String {
    imported::getText().await
}

let file_content = editor::get_text().await;
````

2026-06-04 Wk 23 Thu - 03:26 +03:00

````ts
__wbg_getText_740c526fd4cd4912: function(arg0) {
    console.log(`B00 (j ${JSON.stringify(arg0)})`);
    const ret = editor.getText();
    console.log(`B01 (j ${JSON.stringify(ret)}) (j ${JSON.stringify(wasm.__wbindgen_malloc)}) (j ${JSON.stringify(wasm.__wbindgen_realloc)})`);
    const ptr1 = passStringToWasm0(ret, wasm.__wbindgen_malloc, wasm.__wbindgen_realloc);
    console.log(`B02 (j ${JSON.stringify(ptr1)})`);
    const len1 = WASM_VECTOR_LEN;
    console.log(`B03`);
    getDataViewMemory0().setInt32(arg0 + 4 * 1, len1, true);
    console.log(`B04`);
    getDataViewMemory0().setInt32(arg0 + 4 * 0, ptr1, true);
    console.log(`B05`);
},

````

We hit only up to and including `B01`.

````
[hello plug] B00 (j undefined)
[hello plug] B01 (j {}) (j undefined) (j undefined)
````

One thing to note is we need to be awaiting in the FFI:

````ts
const ret = await editor.getText();
````

but we are not.

We get to `B04` (with the expected page content in `ret`) if we just add `await` to the `editor.getText()` and async to the function `__wbg_getText_740c526fd4cd4912`.

We have already marked this as async on the rust side, but we’re getting all synchronous outputs in the typescript FFI.

2026-06-04 Wk 23 Thu - 04:58 +03:00

https://wasm-bindgen.github.io/wasm-bindgen/reference/js-promises-and-rust-futures.html

I did as they write here, to add `async` to the extern “C”\` declaration, but we’re still not getting what we expect.

It mentions needing a `wasm-bindgen-futures` dependency, which we do have:

````toml
[dependencies]
wasm-bindgen-futures = "0.4.72"
````

2026-06-04 Wk 23 Thu - 05:33 +03:00

Through `cargo expand | less`,

````rust
pub async fn getText() -> String {
    unsafe fn __wbg_getText_740c526fd4cd4912() -> wasm_bindgen::convert::WasmRet<
        <wasm_bindgen_futures::js_sys::Promise<
            String,
        > as wasm_bindgen::convert::FromWasmAbi>::Abi,
    > {
        {
            ::core::panicking::panic_fmt(
                format_args!(
                    "cannot call wasm-bindgen imported functions on non-wasm targets",
                ),
            );
        };
    }
    unsafe {
        let _ret = { __wbg_getText_740c526fd4cd4912() };
        wasm_bindgen_futures::JsFuture::from(
                <wasm_bindgen_futures::js_sys::Promise<
                    String,
                > as wasm_bindgen::convert::FromWasmAbi>::from_abi(
                    _ret.join(),
                ),
            )
            .await
            .expect("uncaught exception")
    }
}
````

If we remove the async in rust for getText:

````rust
#[allow(nonstandard_style)]
#[allow(clippy::all, clippy::nursery, clippy::pedantic, clippy::restriction)]
pub fn getText() -> String {
    unsafe fn __wbg_getText_79377f3d44680bb2() -> wasm_bindgen::convert::WasmRet<
        <String as wasm_bindgen::convert::FromWasmAbi>::Abi,
    > {
        {
            ::core::panicking::panic_fmt(
                format_args!(
                    "cannot call wasm-bindgen imported functions on non-wasm targets",
                ),
            );
        };
    }
    unsafe {
        let _ret = { __wbg_getText_79377f3d44680bb2() };
        <String as wasm_bindgen::convert::FromWasmAbi>::from_abi(_ret.join())
    }
}
````

With this corresponding in `hello_wasm.js`:

````ts
__wbg_getText_79377f3d44680bb2: function(arg0) {
    const ret = editor.getText();
    const ptr1 = passStringToWasm0(ret, wasm.__wbindgen_malloc, wasm.__wbindgen_realloc);
    const len1 = WASM_VECTOR_LEN;
    getDataViewMemory0().setInt32(arg0 + 4 * 1, len1, true);
    getDataViewMemory0().setInt32(arg0 + 4 * 0, ptr1, true);
},
````

`__wbg_getText_79377f3d44680bb2` is pretty much identical to before. Adding or removing `async` will change things in `cargo expand` but not in the generated `hello_wasm.js`.

`./hello-wasm/pkg/hello_wasm.js` is generated via `wasm-pack build --target web`.

https://github.com/wasm-bindgen/wasm-pack

* [gh wasm-bindgen/wasm-pack Build::run](https://github.com/wasm-bindgen/wasm-pack/blob/1d35e8fe1210d2cfe680adf5a0c06fe2d8bfd4a2/src/command/build.rs#L310)
  * [gh wasm-bindgen/wasm-pack cargo_build_wasm](https://github.com/wasm-bindgen/wasm-pack/blob/1d35e8fe1210d2cfe680adf5a0c06fe2d8bfd4a2/src/build/mod.rs#L85)

2026-06-04 Wk 23 Thu - 06:45 +03:00

````sh
# in /home/lan/src/cloned/gh/wasm-bindgen
git clone git@github.com:wasm-bindgen/wasm-pack.git

rustup update
````

2026-06-04 Wk 23 Thu - 06:59 +03:00

I am currently on `wasm-bindgen = "0.2.84"` while the [gh wasm-bindgen wasm-pack](https://github.com/wasm-bindgen/wasm-pack) is pointing at v0.15.0. Upgrading.

````sh
cargo upgrade


# out
    Checking hello-wasm's dependencies
name              old req compatible latest  new req
====              ======= ========== ======  =======
wasm-bindgen      0.2.84  0.2.122    0.2.122 0.2.122
wasm-bindgen-test 0.3.34  0.3.72     0.3.72  0.3.72
   Upgrading recursive dependencies
     Locking 0 packages to latest compatible versions
````

Actually, `wasm-bindgen` is currently at `0.2.122`: https://github.com/wasm-bindgen/wasm-bindgen, to not be confused with wasm-pack.

Version upgrades did not affect output.

But look at https://wasm-bindgen.github.io/wasm-bindgen/api/wasm_bindgen_futures/ :

 > 
 > This crate is now a thin shim re-exporting from [`js_sys::futures`](https://wasm-bindgen.github.io/wasm-bindgen/api/js_sys/futures/index.html "mod js_sys::futures"). The implementation has been moved into `js-sys` so that [`js_sys::Promise`](https://wasm-bindgen.github.io/wasm-bindgen/api/js_sys/struct.Promise.html "struct js_sys::Promise") can implement [`core::future::IntoFuture`](https://doc.rust-lang.org/nightly/core/future/into_future/trait.IntoFuture.html "trait core::future::into_future::IntoFuture") directly, enabling `promise.await` without any wrapper type.

In https://wasm-bindgen.github.io/wasm-bindgen/reference/types/js-sys.html#promiset,

they just type it with `Promise<T>` instead of using `async` on the extern “C” declarations. Let’s try this.

Changing it to `Promise` does make a difference in the generated output:

````ts
__wbg_getText_9f67aa406c2d9281: typeof editor.getText == 'function' ? editor.getText : notDefined('editor.getText'),
````

and it’s not identical; we now have to handle a `Result` after await.

Now we make it further in rust, up to `A13`:

````rust
console_log(&format!("A13"));
let cursor_pos = editor::get_cursor().await as usize;
````

We have yet to make the change for this async `get_cursor()` so this error is expected. Now to make this update to all the API functions.

2026-06-04 Wk 23 Thu - 15:10 +03:00

````sh
# set autoread | au CursorHold * checktime | call feedkeys("lh")

## -- Update extern declares to be Promise<T> instead of async and unwrap in consumer --

cp a b

# Change #[wasm_bindgen(\1)]\n\s*pub async fn \2(\3) -> \4; to
#        #[wasm_bindgen(\1)]\n\s*pub       fn \2(\3) -> Promise<\4>;
sed -zi "s/#\[wasm_bindgen(\([^)]*\))\] *\n *pub async fn \([a-zA-Z0-9_]*\)(\([^)]*\)) -> \([^;]*\);/ \
           #[wasm_bindgen(\1)]\npub fn \2(\3) -> js_sys::Promise<\4>;/g" b

# Change #[wasm_bindgen(\1)]\n\s*pub async fn \2(\3); to
#        #[wasm_bindgen(\1)]\n\s*pub       fn \2(\3) -> Promise<()>;
sed -zi "s/#\[wasm_bindgen(\([^)]*\))\] *\n *pub async fn \([a-zA-Z0-9_]*\)(\([^)]*\));/ \
           #[wasm_bindgen(\1)]\npub fn \2(\3) -> js_sys::Promise<()>;/g" b

# Unwrap all `.await`s in general. If some are expected failures, we will customize.
sed -i "s/.await/.await.unwrap()/g" b
````

2026-06-04 Wk 23 Thu - 15:57 +03:00

We get the current line now!

2026-06-05 Wk 23 Fri - 10:26 +03:00

Spawn [000 Rust Can we process a compile-time serial parallel DAG of tokens?](../investigation/000%20Rust%20Can%20we%20process%20a%20compile-time%20serial%20parallel%20DAG%20of%20tokens%3F.md) ^spawn-invst-d48fec
