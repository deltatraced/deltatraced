---
context_type: issue
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/001 create a rust silverbullet plugin](../task/001%20create%20a%20rust%20silverbullet%20plugin.md)

Spawned in: [^spawn-issue-c8026f](../task/001%20create%20a%20rust%20silverbullet%20plugin.md#spawn-issue-c8026f)

# Journal

2026-05-24 Wk 21 Sun - 11:11 +03:00

Oh oops should look at the new `hello-wasm`,

````sh
wasm-pack build --target web
````

2026-05-24 Wk 22 Sun - 02:50 +03:00

When trying to use `greet` in a command:

````
Error: e.arrayBuffer is not a function
````

2026-05-24 Wk 22 Sun - 03:44 +03:00

* There are some rust templates for obsidian plugins we might get ideas from:
  * [gh rachtsingh/obsidian-rust-template](https://github.com/rachtsingh/obsidian-rust-template)
  * [gh trashhalo/obsidian-rust-plugin](https://github.com/trashhalo/obsidian-rust-plugin)

The exact error happens in formatted line 8942 of some anonymous code:

````js
if (r.breakOnConsoleErrors, l) {
  let t = formatWithStyles(n, c);
  o &&
  (t = [
    `${ t[0] } %o`,
    ...t.slice(1)
  ]),
  e(...t)
} else e(...n)
//     ~ here
````

2026-05-24 Wk 22 Sun - 04:11 +03:00

This error only occurs when we trigger `await init();` This directs to `__wbg_init`.

Maybe this could be related:

[wasm-bindgen.github.io considerations](https://wasm-bindgen.github.io/wasm-pack/book/prerequisites/considerations.html)

From [post wisstudio.com 21179 giri-zano](https://forum.wixstudio.com/t/arraybuffer-is-not-a-function/21179),

 > 
 > Could it be that arrayBuffer is a client-side (browser) concept and backend modules run, by def, on the server? Try Buffer (node.js concept) instead.

2026-05-24 Wk 22 Sun - 04:47 +03:00

For checking runtime information: [gh cross-org/runtime](https://github.com/cross-org/runtime):

````sh
npx jsr add @cross/runtime
````

````ts
import { 
  CurrentArchitecture,
  CurrentOS,
  CurrentProduct,
  CurrentRuntime,
  CurrentVersion,
  Runtime
} from "@cross/runtime";

export async function helloWorld() {
  console.log(`Runtime: ${CurrentRuntime}`);
  console.log(`OS: ${CurrentOS}`);
  console.log(`Architecture: ${CurrentArchitecture}`);
  console.log(`Product: ${CurrentProduct}`);
  console.log(`Version: ${CurrentVersion}\n`);

  if (CurrentRuntime == Runtime.Deno) {
    console.log("You're running Deno!");
  } else {
    console.log("You're not running Deno!");
  }
}
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/clusterlinemd
npm install && npm run build
````

`Ctrl+/ > Plugs: Reload`, `Ctrl+/ > Say Hello`

Then in the console (Ctrl+shift+I, go to Console):

````
[hello plug] Runtime: unsupported
[hello plug] OS: unsupported
[hello plug] Architecture: unsupported
[hello plug] Product: unsupported
[hello plug] Version: 150
[hello plug] You're not running Deno!
````

2026-05-24 Wk 22 Sun - 05:03 +03:00

From `/home/lan/src/cloned/gh/silverbulletmd/silverbullet/website/Plugs/Development.md`,

````
A **plug** is a self-contained JavaScript bundle (`*.plug.js`) that extends SilverBullet. It runs inside a sandboxed [Web Worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers), talks to the editor via [[API|syscalls]], and hooks into SilverBullet through commands, events, slash commands, message queues, and more.
````

From `/home/lan/src/cloned/gh/silverbulletmd/silverbullet/build/build_plug_compile.ts`,

````ts
export async function buildPlugCompile(): Promise<void> {
  await mkdir("dist", { recursive: true });

  // Pre-bundle the worker runtime so it can be embedded into plug-compile.js
  // as a string constant. This way the bundled CLI is fully self-contained.
  const workerBuild = await esbuild.build({
    entryPoints: ["client/plugos/worker_runtime.ts"],
    bundle: true,
    format: "esm",
    platform: "browser",
    write: false,
    treeShaking: true,
  });
  const workerRuntimeJS = workerBuild.outputFiles[0].text;
  // [...]
````

2026-05-24 Wk 22 Sun - 05:23 +03:00

So trying with `wasm-pack build --target bundler` since the above suggests it expects a bundle.

That would give the error

````ts
Building [ 'hello.plug.yaml' ]
✘ [ERROR] No loader is configured for ".wasm" files: hello-wasm/pkg/hello_wasm_bg.wasm

    hello-wasm/pkg/hello_wasm.js:2:22:
      2 │ import * as wasm from "./hello_wasm_bg.wasm";
        ╵                       ~~~~~~~~~~~~~~~~~~~~~~

Error building hello.plug.yaml: Build failed with 1 error:
hello-wasm/pkg/hello_wasm.js:2:22: ERROR: No loader is configured for ".wasm" files: hello-wasm/pkg/hello_wasm_bg.wasm
/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:1748
  let error = new Error(text);
              ^

Error: Build failed with 1 error:
hello-wasm/pkg/hello_wasm.js:2:22: ERROR: No loader is configured for ".wasm" files: hello-wasm/pkg/hello_wasm_bg.wasm
    at failureErrorWithLog (/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:1748:15)
    at /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:1207:25
    at runOnEndCallbacks (/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:1588:45)
    at buildResponseToResult (/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:1205:7)
    at /home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:1232:16
    at responseCallbacks.<computed> (/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:884:9)
    at handleIncomingPacket (/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:939:12)
    at Socket.readFromStdout (/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/node_modules/esbuild/lib/main.js:862:7)
    at Socket.emit (node:events:507:28)
    at addChunk (node:internal/streams/readable:559:12) {
  errors: [Getter/Setter],}
  warnings: [Getter/Setter]
}

Node.js v24.1.0
````

In [gh evanw/esbuild #3114 evanw](https://github.com/evanw/esbuild/issues/3114),

 > 
 > This intentionally isn't something that esbuild automatically does for you. Loading WebAssembly code involves external assets, and how to do that is case-dependent. You need to configure esbuild to tell it how you want it to do that (e.g. by using a plugin) depending on your use case.

Also was shared: [esbuild plugins](https://esbuild.github.io/plugins/).

2026-05-24 Wk 22 Sun - 05:41 +03:00

[gh Tschrock/esbuild-plugin-wasm](https://github.com/Tschrock/esbuild-plugin-wasm)

Might not be something I can use if it is silverbullet.md that controls the esbuild building process, and in there they use no plugins.

* `Plugs: Reload`
  * $\to$ fn reloadPlugsCommand (gh/silverbulletmd/silverbullet/plugs/editor/system.ts)
    * $\to$ fn reloadPlugsFromSpace (gh/silverbulletmd/silverbullet/client/client_system.ts)
      * $\to$ fn loadPlugFromPath (gh/silverbulletmd/silverbullet/client/client_system.ts)

2026-05-24 Wk 22 Sun - 08:01 +03:00

Our own compilation with `npm run build` uses `npx plug-compile *.plug.yaml` in `/home/lan/src/cloned/gh/LanHikari22/clusterlinemd/package.json`

````json
{
  "name": "silverbullet-plug-template",]
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "build": "npx plug-compile hello.plug.yaml"
  },
  "dependencies": {
    "@cross/runtime": "npm:@jsr/cross__runtime@^1.2.1",
    "@silverbulletmd/silverbullet": ">=2.5.3"
  }
}

````

This should correspond to `/home/lan/src/cloned/gh/silverbulletmd/silverbullet/bin/plug-compile.ts`.

So from `runBuildStep` we know this is what runs:

````ts
    await esbuild.build({
      entryPoints: [inFile],
      bundle: true,
      format: "iife",
      platform: "browser",
      minify: !options.debug,
      outfile: outFile,
      treeShaking: true,
    });
````

So `wasm-pack build --target bundle` seems out of question unless we want to use something other than `plug-compile`.

2026-05-24 Wk 22 Sun - 08:59 +03:00

Back to this:

````
[hello plug] An exception was thrown as a result of invoking function helloWorld error: e.arrayBuffer is not a function
````

I see this error submessage `An exception was thrown as a result of invoking function` in the compiled `hello.plug.js`, and it matches the function `setupMessageListener` from `worker-runtime`,

This is `/home/lan/src/cloned/gh/silverbulletmd/silverbullet/client/plugos/worker_runtime.ts`.

::Aside
`runningAsWebWorker` in worker_runtime.ts is a test the silverbullet dev team used to distinguish the js runtime environment.
::

Further, modifying my `hello.plug.js` error message resulted in a modification in invocation, so we have confirmation that is the code currently running and breaking:

````
[hello plug] An exception (right?) was thrown as a result of invoking function helloWorld error: e.arrayBuffer is not a function
````

````js
console.log("A0");
let a = await Promise.resolve(s(...(i.args || [])));
console.log("A1");
d({ type: "invr", id: i.id, result: a });
console.log("A2");
````

Only `A0` triggers, so the failure is in computing `a`.

````js
console.log("B0");
let a0 = s(...(i.args || []));
console.log("B1");
let a = await Promise.resolve(a0);
console.log("B2");
d({ type: "invr", id: i.id, result: a });
console.log("B3");
````

`B0, B1` trigger. The issue is in resolving the promise.

`s` is our command:

````
[hello plug] B0 (args [object Object]) (s async function w() {
  (await l.flashNotification("It is you typescript..."),
    await v(),
    await l.flashNotification(`The answer is ${b(2, 9)}`));
})
````

We never get this far:

````js
function b(e, r) {
  console.log("A00")
  return c.calc(e, r) >>> 0;
}
````

````js
async function v(e) {
  console.log("A20");
  if (c !== void 0) return c;
  console.log("A21");
  (e !== void 0 &&
    (Object.getPrototypeOf(e) === Object.prototype
      ? ({ module_or_path: e } = e)
      : console.warn(
          "using deprecated parameters for the initialization function; pass a single object instead",
        )),
    e === void 0 && (e = new URL("hello_wasm_bg.wasm", import.meta.url)));
  console.log("A22");
  let r = Vt();
  console.log("A23");
  (typeof e == "string" ||
    (typeof Request == "function" && e instanceof Request) ||
    (typeof URL == "function" && e instanceof URL)) &&
    (e = fetch(e));
  console.log("A24");
  let { instance: o, module: n } = await It(await e, r);
  console.log("A25");
  return zt(o, n);
}
````

We get all the way up to `A24`. We never makesw it to `It`. It stops in `B24`:

````js
  console.log("B24");
  let result1 = await e;
  console.log("B25");
  let { instance: o, module: n } = await It(result1, r);
````

````js
  let r = Vt();
  console.log(`A23 ${JSON.stringify(e)} ${JSON.stringify(r)}`);
  (typeof e == "string" ||
    (typeof Request == "function" && e instanceof Request) ||
    (typeof URL == "function" && e instanceof URL)) &&
    (e = fetch(e));
  console.log(`A24 ${e} ${JSON.stringify(e)} ${typeof e}`);
  let result1 = await e;
````

````
[hello plug] A23 "http://localhost:3000/.fs/Library/LanHikari22/clusterlinemd/hello_wasm_bg.wasm" {"./hello_wasm_bg.js":{}} hello.plug.js:41:35
[hello plug] A24 [object Promise] {} object
````

It seems `fetch` gave us an empty object.

````js
  console.log(`B21 ${JSON.stringify(e)}`);
  (e !== void 0 &&
    (Object.getPrototypeOf(e) === Object.prototype
      ? ({ module_or_path: e } = e)
      : console.warn(
          "using deprecated parameters for the initialization function; pass a single object instead",
        )),
    e === void 0 && (e = new URL("hello_wasm_bg.wasm", import.meta.url)));
  console.log(`B22 ${JSON.stringify(e)}`);
````

````
[hello plug] B21 undefined
[hello plug] B22 "http://localhost:3000/.fs/Library/LanHikari22/clusterlinemd/hello_wasm_bg.wasm"
````

2026-05-24 Wk 22 Sun - 10:40 +03:00

Anyway the main issue is that `fetch` is stubbed out in this scripting environment. We need to encode the wasm data as base64 and embed it ourselves, without using fetch.

This was suggested in [gh wasm-bindgen/wasm-pack #831 ](https://github.com/wasm-bindgen/wasm-pack/issues/831) but the issue is still open. A user there suggested [gh jeff-hykin/wasm-pack-embed-unofficial](https://github.com/jeff-hykin/wasm-pack-embed-unofficial) with instructions

````sh
# install deno (if you don't have it)
curl -fsSL https://deno.land/install.sh | sh

# install this tool (wpe)
deno install -n wpe -Afg https://esm.sh/gh/jeff-hykin/wasm-pack-embed-unofficial@1.0.0.7/wpe.js

# call wasm-pack and wpe
wasm-pack build --target web && wpe --output-folder ./pkg
````

2026-05-24 Wk 22 Sun - 10:53 +03:00

````
[hello plug] An exception was thrown as a result of invoking function helloWorld error: can't access property "calc", Gt is undefined
````

When we instead use the embedded:

````ts
import { editor } from "@silverbulletmd/silverbullet/syscalls";
import { calc } from "./hello-wasm/pkg/main_embedded.js"

export async function helloWorld() {
  await editor.flashNotification("It is you typescript...");

  await editor.flashNotification(`The answer is ${calc(2, 9)}`);
}
````

2026-05-24 Wk 22 Sun - 10:56 +03:00

But this actually works:

````ts
import { editor } from "@silverbulletmd/silverbullet/syscalls";

import wasmAsUtf8Array from "./hello-wasm/pkg/wasm_bytes.js"
// manually load it into the wasm-pack module
import { initSync, calc } from "./hello-wasm/pkg/hello_wasm.js"


export async function helloWorld() {
  await editor.flashNotification("It is you typescript...");

  await initSync({module:wasmAsUtf8Array});

  await editor.flashNotification(`The answer is ${calc(2, 9)}`);
````

We get a prompt:

````
The answer is 20
````

Though you don’t need that `await` in `await initSync`. It’s synchronous.
