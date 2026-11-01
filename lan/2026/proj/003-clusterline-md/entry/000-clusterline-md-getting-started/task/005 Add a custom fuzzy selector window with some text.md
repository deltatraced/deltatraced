---
context_type: task
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned in: [^spawn-task-3ab4dd](../000-clusterline-md-getting-started.md#spawn-task-3ab4dd)

Overview: [000 Overview clusterline getting started](../entry/000%20Overview%20clusterline%20getting%20started.md)

Goal: [000 Overview Goals for clusterlinemd > Version 0.1 Initial Functionality](../../001%20Goals%20for%20clusterlinemd/entry/000%20Overview%20Goals%20for%20clusterlinemd.md#version-01-initial-functionality)

# Journal

## Journal

2026-06-15 Wk 25 Mon - 10:28 +03:00

We need to be able to open a menu similar to Ctrl+K

 > 
 > **aside**
 > It would be nice if we could actually interact with the mentions.
 > 
 > A hack I do for now is having vimium-c on firefox with the filter `^ f` to only allow f. To get the focus out of the editor, ctrl+, then escape, which opens config and leaves, and gives no focus to the editor. Then you can use f.
 > 
 > Adding `^ f j k` gives even more smooth quick motion. You can get back to the editor with Ctrl+/ Escape.
 > 
 > Of course, proper link hint following in a plug would be optimal.

2026-06-15 Wk 25 Mon - 12:43 +03:00

Let’s look at the command `Open Command Palette` in the silverbullet [source](https://github.com/silverbulletmd/silverbullet). This points to `client.startCommandPalette()`.

````ts
// in client/client.ts
  async startCommandPalette() {
    const commands = this.ui.viewState.commands;
    await this.commandAugmenter.augmentObjectMap(commands);
    this.ui.viewDispatch({
      type: "show-palette",
      commands,
      context: client.getContext(),
    });
  }
````

They also do not expose `this.ui` directly, instead they expose

````ts
// in plug-api/syscalls/editor.ts
/**
 * Dispatch a CodeMirror transaction: https://codemirror.net/docs/ref/#state.Transaction
 */
export function dispatch(change: any): Promise<void> {
  return syscall("editor.dispatch", change);
}

// in client/plugos/syscalls/editor.ts > fn editorSyscalls
    "editor.dispatch": (_ctx, change: Transaction) => {
      client.editorView.dispatch(change);
    },
````

So let's see how they actually implement `show-palette`. This is the action through the reducer for `viewDispatch`:

````ts
// in client/reducer.ts > fn reducer
    case "show-palette": {
      return {
        ...state,
        showCommandPalette: true,
        showPageNavigator: false,
        showFilterBox: false,
        showCommandPaletteContext: action.context,
        commands: action.commands,
      };
    }
````

It's simply a part of the `MainUI` generated html.

````ts
// in client/editor_ui.tsx > fn ViewComponent
        {viewState.showCommandPalette && (
          <CommandPalette
			  // ...
          />
        )}
````

So no hope for a plug to define its own fuzzy filter window through this. But is it possible for a plug to create its own html elements to add?

They also mention that we can create widgets with space-lua in the README.

https://silverbullet.md/Architecture

Some plugs we've seen make major changes to the UI itself. Let's take a look at

https://silverbullet.md/Plugs $\to$ https://github.com/MrMugame/silversearch

This obviously has a fuzzy window, so how did they do it?

https://github.com/MrMugame/silversearch/blob/main/modal/modal.ts

So they have access to `document` directly, and they mount to `#container` which we can find in silverbullet `plugs/image-viewer/viewer.ts`.

This mounting action is described here: https://svelte.dev/docs/svelte/imperative-component-api

2026-06-15 Wk 25 Mon - 18:07 +03:00

But I don't have access to `document` in typescript via the plug:

````ts
// in src/hello.ts
export async function helloWorld() {
  console.log(`AAA00 (doc ${JSON.stringify(document.URL)})`);
}
````

````
[hello plug] An exception was thrown as a result of invoking function helloWorld error: document is not defined
````

Actually in [here](https://github.com/MrMugame/silversearch/blob/b33127a907db5a46926f9b569f2e3ae4f2d61f64/worker/silversearch.ts#L50) they're using `editor.showPanel` to pass in the html:

````ts
export async function openSearch(defaultQuery: string  = ""): Promise<void> {
    await editor.showPanel(
        "modal",
        // We can't have a falsy value (0) here, because of some silverbullet oddities
        1,
        html,
        defaultQuery ? `globalThis.DEFAULT_QUERY = ${JSON.stringify(defaultQuery)};` + script : script,
    );
}
````

`html` and `script` being prebuilt from `modal` we've seen before.

So we need to be able to build `HTMLElements` in rust too.

https://book.leptos.dev/web_sys.html

This seems to be fairly similar to preact with defining components.

https://github.com/leptos-rs/leptos

https://book.leptos.dev/appendix_reactive_graph.html

2026-06-15 Wk 25 Mon - 22:43 +03:00

The basic example doesn't work out of the box, but shouldn't jump to make an issue for this. This seems to be a nightly vs stable semantics issue: https://github.com/leptos-rs/book/issues/54

````rust
use leptos::prelude::*;

#[component]
fn App() -> impl IntoView {
    let (count, set_count) = signal(0);

    view! {
        <button
            on:click=move |_| set_count.set(3)
        >
            "Click me: "
            {count}
        </button>
        <p>
            "Double count: "
            {move || count.get() * 2}
        </p>
    }
}
````

2026-06-17 Wk 25 Wed - 10:50 +03:00

Ran into [000 WESR leptos mismatches types on lower edition](../../../../../topic/logs/000%20Records/wikiproc/000%20Wiki%20Proc%20Error%20Source%20Records/entry/000%20WESR%20leptos%20mismatches%20types%20on%20lower%20edition.md)

But changing edition to `2021` now is giving us a lot of errors in the API, so I guess at least that's good. We should be compatible with newest.

2026-06-18 Wk 25 Thu - 13:36 +03:00

Actually the errors were just because with changing `Cargo.toml`, I had set wasm-bindgen as optional, reverted back to required and all is good.

2026-06-21 Wk 25 Sun - 10:28 +03:00

We're able to pass `html` and `script` like this:

````rust
let html = r#"client/plugos/hooks/syscall.ts
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
};
"#;

    editor::show_panel(PanelLocation::Lhs, /*mode*/ inv(U32Nz::new(1)), html, script).await;
````

but unsure if we can do this with leptos. Maybe we need to have its own project generate a full client that doesn't need to be served, and then we pass this over in the plug code.

https://github.com/leptos-rs/leptos/tree/main/examples/counter

````sh
mkdir -p ~/src/cloned/gh/leptos-rs
cd ~/src/cloned/gh/leptos-rs
git clone git@github.com:leptos-rs/leptos.git

# in /home/lan/src/cloned/gh/leptos-rs/leptos/examples/counter
cargo install cargo-make
cargo install trunk
cargo make start
cargo make stop
````

This will generate files in `dist/` but they can't be run as is off of `index.html`.

Also the purpose of trying to use `leptos` here is to have the html to be run by silverbullet responsive to the same rust code we are writing for the plug.

We might need to make it responsive by having the script execute plug commands instead via syscall, which in turn generates a new panel to be displayed. Or use `src/silverbullet_plug_api/system.rs > fn invoke_function`.
client/plugos/hooks/syscall.ts
2026-06-21 Wk 25 Sun - 18:55 +03:00

We get this error trying to call via syscall:

````ts
// in client/plugos/system.ts > fn syscall
if (!syscall) {
  throw Error(`Unregistered syscall ${name}`);
}
````

Spawn [002 SB How are plug functions registered as a syscall? 13de05c8](../investigation/002%20SB%20How%20are%20plug%20functions%20registered%20as%20a%20syscall%3F%2013de05c8.md) ^spawn-invst-618590

2026-06-30 Wk 27 Tue - 21:04 +03:00

Spawn [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/issue/002 SB Unregistered syscall while trying to trigger plug command defined in plug yaml](../issue/002%20SB%20Unregistered%20syscall%20while%20trying%20to%20trigger%20plug%20command%20defined%20in%20plug%20yaml.md) ^spawn-issue-a3158e

2026-07-01 Wk 27 Wed - 01:26 +03:00

So we wanna create a non-user-command plug function `post_message`, where we can specify a topic; a subtopic, and comma separated arguments. When we send. The html and javascript we send over to silverbullet can then drive communications back via message posts, and different subtopics can be for things like the panels being closed, or the html interacted with in some way.

Actually. Better than comma separated args is a JSON. That should be very flexible. Although there isn't really a message broker or publisher-subscriber infrastructure. We just basically have a similar `topic/subtopic/json message` data format.

2026-07-01 Wk 27 Wed - 01:51 +03:00

````
[clusterline plug] AAA00.00
[clusterline plug] AAA00.01 panel,btn1_onclick,{} undefined undefined
[clusterline plug] An exception was thrown as a result of invoking function post_message error: r.charCodeAt is not a function
````

````ts
// Panel Code
let count = 0;
document.getElementById('btn1').onclick = () => {
    count++;
    document.getElementById('count').textContent = count;
    syscall('system.invokeFunction', 'clusterline.post_message', ['panel', 'btn1_onclick', '{}']);
};

// Plug ts code
export async function ts_post_message(topic: string, subtopic: string, json_msg: string) {
  console.log(`AAA00.00`);
  initSync({ module: wasmAsUtf8Array });
  console.log(`AAA00.01 ${topic} ${subtopic} ${json_msg}`);
  let result = await post_message(topic, subtopic, json_msg);
  console.log(`AAA00.02 ${result}`);
  return result;
}
````

The `AAA00.01` log shows that the arguments are passed all in `topic` as comma separated.

The first argument also seems to just come as an object:

````ts
export async function ts_post_message(comma_separated_args: string) {
  console.log(`AAA00.00 (csa ${comma_separated_args}) (type ${typeof(comma_separated_args)})`);
````

````
[clusterline plug] AAA00.00 (csa panel,btn1_onclick,{}) (type object)
````

so we end up unable to split:

````
[clusterline plug] An exception was thrown as a result of invoking function post_message error: r.split is not a function
````

Explicit casting to String with `String(theObj)` works for this. And we are able to get the message recognized from the plug in rust!

2026-07-01 Wk 27 Wed - 18:48 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterlinemd
git commit

# out
[main 64a04c8] impl basic message post for ui bundle
````

The UI Bundle (html and script) now can communicate back with the rust plug in a modular way with topics and subtopics. It can receive a result from its message post, allowing computation to be done over in rust also to be used by the ui bundle. The UI Bundle includes a service string that the rust plug can use to route to the originator of that UI bundle to handle interaction. This way the same plug can have multiple services that make use of modal selector UI, etc.

2026-07-01 Wk 27 Wed - 19:08 +03:00

Now we have the communications and ability to spawn our own UI with a plug. Next, we want to be able to offer a similar experience to silverbullet with our modals if possible.

the webcomponent `client/components/command_palette.tsx > CommandPalette` is basically a specialization of `client/components/filter.tsx > FilterList`. As we are going to be clone or near clone of this, ensure the file pays respect to the silverbullet license.

Spawn [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/entry/001 Silverbullet Filterlist html notes](../entry/001%20Silverbullet%20Filterlist%20html%20notes.md) ^spawn-entry-30bc97

2026-07-03 Wk 27 Fri - 22:51 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterlinemd
git commit

# out
[main 9624eec] add clusterline-ui subproject for building panel UI to be sent to silverbullet
````

We have a basic similar, although it does not yet look identical, UI for the modal options filter. We have a new project `clusterline-ui` for building these widgets so that we can send the html over for silverbullet, although this pipeline is yet to be integrated.

This project uses the same build process we achieved in [001 Setup repository build PPST](../../../../../microproj/001-ping-pong-score-ts/entry/000%20Getting%20started%20ping-pong-score-ts/task/001%20Setup%20repository%20build%20PPST.md), which includes `scss` and `ts` in the build. Silverbullet uses `scss` as well so we are able to use the same styles.

![Pasted image 20260703225935.png](../../../../../../../attachments/Pasted%20image%2020260703225935.png)

## Tasks

2026-07-04 Wk 27 Sat - 15:33 +03:00

* [x] [007 Integrate ui build with rust plug and send proper modal dialog to silverbullet](007%20Integrate%20ui%20build%20with%20rust%20plug%20and%20send%20proper%20modal%20dialog%20to%20silverbullet.md)
* [x] [008 Allow multiple modal UI to be built and integrated with rust plug Clusterline](008%20Allow%20multiple%20modal%20UI%20to%20be%20built%20and%20integrated%20with%20rust%20plug%20Clusterline.md)
* [ ] [009 Include rust plug post message and render config in clusterline-ui and stub message post in test build](009%20Include%20rust%20plug%20post%20message%20and%20render%20config%20in%20clusterline-ui%20and%20stub%20message%20post%20in%20test%20build.md)
* [ ] Implement behavior for selecting item, filtering item via search, responding to keyUp and keyDown
* [ ] Improve style so that the dialog window is smaller, and the scroll is inside the result list div, and the hints are far right
* [ ] Implement fuzzy search functionality through the input field. Include `!` syntax for filtering keywords.

## Journal

2026-07-04 Wk 27 Sat - 19:00 +03:00

Spawn [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/007 Integrate ui build with rust plug and send proper modal dialog to silverbullet](007%20Integrate%20ui%20build%20with%20rust%20plug%20and%20send%20proper%20modal%20dialog%20to%20silverbullet.md) ^spawn-task-8017b4

2026-07-05 Wk 27 Sun - 05:36 +03:00

Spawn [008 Allow multiple modal UI to be built and integrated with rust plug Clusterline](008%20Allow%20multiple%20modal%20UI%20to%20be%20built%20and%20integrated%20with%20rust%20plug%20Clusterline.md) ^spawn-task-0f48e4

2026-07-05 Wk 27 Sun - 07:51 +03:00

Spawn [009 Include rust plug post message and render config in clusterline-ui and stub message post in test build](009%20Include%20rust%20plug%20post%20message%20and%20render%20config%20in%20clusterline-ui%20and%20stub%20message%20post%20in%20test%20build.md) ^spawn-task-1070ec

2026-07-07 Wk 28 Tue - 17:56 +03:00

Spawn [011 Impl sb_options_filter_list interactive behavior](011%20Impl%20sb_options_filter_list%20interactive%20behavior.md) ^spawn-task-3c7bf1

Spawn [012 Style sb_options_filter_list](012%20Style%20sb_options_filter_list.md) ^spawn-task-fd6ab5
