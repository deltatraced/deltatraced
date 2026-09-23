---
context_type: issue
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/entry/002 Issues for Include rust plug post message in clusterline-ui and replace it with logs in test build](../entry/002%20Issues%20for%20Include%20rust%20plug%20post%20message%20in%20clusterline-ui%20and%20replace%20it%20with%20logs%20in%20test%20build.md)

Spawned in: [^spawn-issue-96f454](../entry/002%20Issues%20for%20Include%20rust%20plug%20post%20message%20in%20clusterline-ui%20and%20replace%20it%20with%20logs%20in%20test%20build.md#spawn-issue-96f454)

# Inference

$\therefore$ Futures in rust must be passed to an executor or awaited, or the computation is never carried out. The difference in this issue was that we ignored a future, and so our action became a no-op.

# Journal

2026-07-06 Wk 28 Mon - 06:55 +03:00

This doesn't close the modal dialog, but pressing ESC another time will print the same logs then close it, but when we call `greet` again it will not show. In fact this time we lose control of the editor and can't get back to it.

When disabling `let _ = editor::hide_panel(PanelLocation\:\:Modal);` in `on_canceled_json`, then a single `Esc` closes the modal.

`/html/body/div[1]/div[3]/div/iframe` shows under `#document` in the silverbullet runtime DOM the ui bundle html injected, including the hidden tag with the initialized JSON. This data was not removed or replaced after cancellation.

Removing the `hidden` from the rendered config shows we can still interact with it after the modal panel was closed.

2026-07-06 Wk 28 Mon - 11:23 +03:00

https://github.com/MrMugame/silversearch/

From `modal/components/ModalContainer.svelte`,

````svelte
<dialog
    class="sb-modal-box"
    oncancel={(e: Event) => {
        e.preventDefault();
        syscall("editor.hidePanel", "modal");
    }}
````

`editor.hidePanel` is called from within the UI bundle code. But does it matter?

Yes. Doing this, now we're able to bring up the modal repeatedly. The remaining code (`post_message`) is also no longer executed, suggesting the signal likely halted execution of the script.

Prior, a `<div class="sb-modal"` was created to host the given ui bundle. Now, it successfully removes it too.

Let's make it so we post a message and then hide the panel in typescript.

Is there any indication that we must call `editor.hidePanel` in the panel code rather than through the plug? We call `showPanel` through the plug.

 > 
 > **aside** silverbullet `plugs/object-graph/ui/use_escape.ts > fn useEscape` is a useful reference for canceling with escape.

It doesn't throw an exception, and documentation is not clear about any such discrepancy of environments.

Adding a `Clusterline: test` command with the content

````ts
export async function ts_test() {
  console.log("hiding panel");

  syscall("editor.hidePanel", "modal");
}
````

to try to hide it through the plug independent of cancel.

Even though neither the rust plug nor the UI bundle call `hidePanel`, it suffices for the plug in the typescript side to call it and `(div id="sb-root") > (div class="sb-modal")` will be removed, and the command `greet` will be able to start the modal again. This is the expected behavior, just like when the UI calls it. So the issue must happen between the typescript $\leftrightarrow$ rust interop.

Let's try to do this with the test command over in the rust side:

````rust
// in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-rs/src/lib.rs
#[wasm_bindgen]
pub async fn test() {
    init();

    let _ = editor::hide_panel(PanelLocation::Modal);
}
````

This reproduces the problem, it is unable to cause `(div id="sb-root")` to be removed.

The problem fails to be reproduced with

````ts
// in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-rs/src/lib.rs
#[wasm_bindgen]
pub async fn test() {
    init();

    editor::hide_panel(PanelLocation::Modal).await;
}
````

--/ 2026-07-07 Wk 28 Tue - 07:42 +03:00

Aside

https://rustc-dev-guide.rust-lang.org/mir/index.html

--/

https://docs.rs/futures/latest/futures/

We don't await; so nothing happens. The future is like a certificate of a task to be executed eventually by a task system. This would imply that discarding the future should resulted in a no-op.

OK
