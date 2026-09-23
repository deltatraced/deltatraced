---
context_type: issue
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/005 Add a custom fuzzy selector window with some text](../task/005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md)

Spawned in: [^spawn-issue-a3158e](../task/005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md#spawn-issue-a3158e)

# Resolution

I called the function wrong within the script passed to silverbullet. This works:

````ts
syscall('system.invokeFunction', 'clusterline.cmd_aaa', []);
````

# Journal

2026-06-30 Wk 27 Tue - 21:04 +03:00

We get the error

````
Uncaught Error: Unregistered syscall clusterline.cmd_aaa
````

and also

````
Uncaught Error: Unregistered syscall clusterline.ts_aaa
````

as we vary tying `cmd_aaa` and `ts_aaa` via this rust code:

````rust
#[wasm_bindgen]
pub async fn greet() {
    init();

    flash_notification("(0) Rust greets you!", NotificationType::Info).await;

    let html = r#"
<div>
  <p>Hi!</p>
  <button id="btn">Click me: <span id="count">0</span></button>
</div>
"#;

    let script = r#"
let count = 0;
document.getElementById('btn').onclick = () => {
    count++;
    document.getElementById('count').textContent = count;
    syscall('clusterline.ts_aaa');
};
"#;

    editor::show_panel(PanelLocation::Lhs, /*mode*/ inv(U32Nz::new(1)), html, script).await;
}

#[wasm_bindgen]
pub async fn test() {
    init();

    flash_notification("Test test!", NotificationType::Info).await;
}
````

and this `clusterline.plug.yaml` portion:

````sh
  greet:
    path: src/clusterline.ts:ts_greet
    command:
      name: "Clusterline: greet"
  cmd_aaa:
    path: src/clusterline.ts:ts_aaa
    command:
      name: "Clusterline: aaa"
````

2026-06-30 Wk 27 Tue - 21:22 +03:00

Trying

````rust
#[wasm_bindgen]
pub async fn greet() {
    init();

    flash_notification("(1) Rust greets you!", NotificationType::Info).await;

    system::invoke_function("ts_aaa", &vec![]).await;
````

````
[clusterline plug] panicked at src/silverbullet_plug_api/system.rs:60:48: called `Result::unwrap()` on an `Err` value: JsValue(Error: Invalid function name ts_aaa R/</<@http://localhost:3000/.fs/Library/lan22h/clusterlinemd/clusterline.plug.js:1:2403 R/<@http://localhost:3000/.fs/Library/lan22h/clusterlinemd/clusterline.plug.js:1:2451 )
````

````rust
#[wasm_bindgen]
pub async fn greet() {
    init();

    flash_notification("(1) Rust greets you!", NotificationType::Info).await;

    system::invoke_function("cmd_aaa", &vec![]).await;
````

````
[clusterline plug] panicked at src/silverbullet_plug_api/system.rs:60:48: called `Result::unwrap()` on an `Err` value: JsValue(Error: Invalid function name cmd_aaa R/</<@http://localhost:3000/.fs/Library/lan22h/clusterlinemd/clusterline.plug.js:1:2403 R/<@http://localhost:3000/.fs/Library/lan22h/clusterlinemd/clusterline.plug.js:1:2451 )
````

Ok these errors are due to the form. We should give `plug.function` argument not `function`. We routed some of those errors via rust.

````sh
#[wasm_bindgen]
pub async fn greet() {
    init();

    flash_notification("(1) Rust greets you!", NotificationType::Info).await;

    system::invoke_function("clusterline.cmd_aaa", &vec![]).await.unwrap();
````

This works!

So the issue is with the environment of the passed javascript for the panel then?

2026-07-01 Wk 27 Wed - 01:04 +03:00

````rust
#[wasm_bindgen]
pub async fn greet() {
    init();

    flash_notification("(1) Rust greets you!", NotificationType::Info).await;

    let html = r#"
<div>
  <p>Hi!</p>
  <button id="btn">Click me: <span id="count">0</span></button>
</div>
"#;

    let script = r#"
let count = 0;
document.getElementById('btn').onclick = () => {
    count++;
    document.getElementById('count').textContent = count;
    syscall('clusterline.cmd_aaa');
};
"#;

    editor::show_panel(PanelLocation::Lhs, /*mode*/ inv(U32Nz::new(1)), html, script).await;
}
````

This produces no errors on click. It doesn't trigger neither of these:

````rust
#[wasm_bindgen]
pub async fn test() {
    init();

    flash_notification("Test test!", NotificationType::Info).await;
    console_log(&format!("You clicked test!"));
}
````

but it does increment the counter.

No, the logger window was stale. We get an error:

````
Uncaught Error: Unregistered syscall clusterline.cmd_aaa
````

We want to invoke a function call and pass this to it.

````diff
-syscall('clusterline.cmd_aaa');
+syscall('invokeFunction', 'clusterline.cmd_aaa', []);
````

````
Uncaught Error: Unregistered syscall invokeFunction
````

We also get all the `AAA00.00, AAA00.01, AAA00.02` logs in

````ts
let count = 0;
document.getElementById('btn').onclick = () => {
    count++;
    console.log("AAA00.00");
    document.getElementById('count').textContent = count;
    console.log("AAA00.01");
    syscall('invokeFunction', 'clusterline.cmd_aaa', []);
    console.log("AAA00.02");
};
````

It's `"system.invokeFunction"`:

````diff
-syscall('invokeFunction', 'clusterline.cmd_aaa', []);
+syscall('system.invokeFunction', 'clusterline.cmd_aaa', []);
````

It works! So it was mostly a usage issue on our end.
