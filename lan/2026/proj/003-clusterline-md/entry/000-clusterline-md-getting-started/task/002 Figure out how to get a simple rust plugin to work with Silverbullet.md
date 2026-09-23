---
context_type: task
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/001 create a rust silverbullet plugin](001%20create%20a%20rust%20silverbullet%20plugin.md)

Spawned in: [^spawn-task-eac5da](001%20create%20a%20rust%20silverbullet%20plugin.md#spawn-task-eac5da)

# Journal

2026-05-23 Wk 22 Sat - 10:33 +03:00

We can try using this template [gh rustwasm/wasm-pack-template](https://github.com/rustwasm/wasm-pack-template) to compile rust to webasm, but the last time it was updated was 3 years ago. We’ll at least need to get higher versions of its packages.

The whole org was archived. They talk about it [here](https://blog.rust-lang.org/inside-rust/2025/07/21/sunsetting-the-rustwasm-github-org/).

They moved a bunch of projects to other orgs. Like [gh wasm-bindgen](https://github.com/wasm-bindgen).

2026-05-23 Wk 22 Sat - 10:47 +03:00

https://wasm-bindgen.github.io/wasm-pack/book/quickstart.html

````sh
curl https://wasm-bindgen.github.io/wasm-pack/installer/init.sh -sSf | sh
````

`wasm-pack new hello-wasm` is a better/more up to date way to generate a template.

2026-05-24 Wk 22 Sun - 02:21 +03:00

The spawn in-page links no longer work. We can only use `#` for headers now it seems. Might be something to look into if we can get that back.

````
feature `default` includes `console_error_panic_hook` which is neither a dependency nor another feature
````

2026-05-24 Wk 21 Sun - 11:10 +03:00

Spawn [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/issue/000 Resolve wasm-pack generated code not fetching in the plugin](../issue/000%20Resolve%20wasm-pack%20generated%20code%20not%20fetching%20in%20the%20plugin.md) ^spawn-issue-c8026f

2026-05-24 Wk 21 Sun - 11:11 +03:00

Awesome! We're closer to being able to make a rust plugin template for silverbullet.md. Next we need interop the other way, we need rust to access the syscalls from

````ts
import { editor } from "@silverbulletmd/silverbullet/syscalls";
````

---

2026-05-24 Wk 22 Sun - 20:39 +03:00

* https://wasm-bindgen.github.io/wasm-bindgen/reference/arbitrary-data-with-serde.html
* https://wasm-bindgen.github.io/wasm-bindgen/reference/attributes/on-js-imports/module.html
* https://wasm-bindgen.github.io/wasm-bindgen/examples/import-js.html
  * Basic importing of functions from js and class methods

---

2026-05-25 Wk 22 Mon - 01:37 +03:00

For the following,

````js
import { editor } from "@silverbulletmd/silverbullet/syscalls";

// later using `editor.flushNotification("Hello!")``
````

We can do

````rs
use wasm_bindgen::prelude::*;

#[wasm_bindgen(module = "@silverbulletmd/silverbullet/syscalls")]
extern "C" {
    #[wasm_bindgen(js_namespace = "editor")]
    pub fn flushNotification(message: &str, typ: JsValue) -> JsValue;
}
````

But using this we get the error

````
[hello plug] An exception was thrown as a result of invoking function helloWorld error: d.flushNotification is not a function
````

-- 2026-05-25 Wk 22 Mon - 02:47 +03:00

Issue should be in http://localhost:3000/.fs/Library/LanHikari22/clusterlinemd/hello.plug.js

````js
__wbg_flushNotification_9e93eb75ff3727d2:function(t,o,n){return d.flushNotification(L(t,o),n)},
````

corresponding to

````js
// in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello.plug.js
__wbg_flushNotification_9e93eb75ff3727d2: function (t, o, n) {
  return l.flushNotification(L(t, o), n);
},
````

-- 2026-05-25 Wk 22 Mon - 02:53 +03:00

````js
__wbg_flushNotification_9e93eb75ff3727d2: function (t, o, n) {
  console.log(`A00 (j ${JSON.stringify(t)}) (j ${JSON.stringify(o)}) (j ${JSON.stringify(n)})`);
  let result1 = L(t, o);
  console.log(`A01 (j ${JSON.stringify(result1)})`);
  let result = l.flushNotification(result1, n);
  console.log(`A02 (j ${JSON.stringify(l)}) (j ${JSON.stringify(result)})`);
  return result;
},
````

````
[hello plug] A00 (j 1048580) (j 17) (j "info") wasm_bytes.js:66:28
[hello plug] A01 (j "Does this work???")
````

Stops at `A01`.

-- 2026-05-25 Wk 22 Mon - 03:16 +03:00

````js
// in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello-wasm/pkg/hello_wasm.js
__wbg_flushNotification_9e93eb75ff3727d2: function(arg0, arg1, arg2) {
    const ret = editor.flushNotification(getStringFromWasm0(arg0, arg1), arg2);
    return ret;
},
````

I noticed that intellisense is not able to tell what `flushNotification` is here, like it is able under `hello.ts`.

-- 2026-05-25 Wk 22 Mon - 04:20 +03:00

We’re able to get a print with

````rust
// in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello-wasm/src/silverbullet_syscalls/editor.rs
#[wasm_bindgen(module = "exported")]
extern "C" {
    pub fn my_print(s: &str) -> JsValue;
}

// in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/hello-wasm/src/lib.rs
use crate::silverbullet_syscalls::editor::{flashNotification, my_print};

#[wasm_bindgen]
pub fn greet() {
    my_print("Does this work???");
}
````

````ts
// in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/src/exported.ts
import { editor } from "@silverbulletmd/silverbullet/syscalls";

export async function my_print(s: string) {
    await editor.flashNotification(s);
}

// in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/src/hello.ts
import wasmAsUtf8Array from "../hello-wasm/pkg/wasm_bytes.js";
import { initSync, calc, greet } from "../hello-wasm/pkg/hello_wasm.js";
import { my_print } from "./exported.js";

export async function helloWorld() {
  initSync({ module: wasmAsUtf8Array });

  greet();
}
````

-- 2026-05-25 Wk 22 Mon - 04:38 +03:00

Nevermind. It was just a typo: `flushNotification` $\to$ `flashNotification`. I thought it was `editor.` that had problems, but actually it just is that `editor` does not have `flushNotification`.

---

::Aside
To be able to manipulate `*.wasm` and disassemble them: https://github.com/WebAssembly/wabt

````sh
sudo apt-get install wabt

# in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd
wasm-objdump -d hello-wasm/pkg/hello_wasm_bg.wasm
````

We can see for example `00374e func[42] <greet>` calls `./hello_wasm_bg.js.__wbg_flushNotification_9e93eb75ff3727d2`. Although we find that in `hello_wasm.js` as `__wbg_flushNotification_9e93eb75ff3727d2`, but at the end of that section it does say `“./hello_wasm_bg.js”: import0,`.

* Spec: https://webassembly.github.io/spec/core/
* Instruction Set: https://webassembly.github.io/spec/core/appendix/index-instructions.html#index-instr

::

2026-05-25 Wk 22 Mon - 02:14 +03:00

Renamed from `Attempt to X` to `X`, since we are able to make a rust silverbullet plugin.
