---
context_type: task
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/005 Add a custom fuzzy selector window with some text](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md)

Spawned in: [^spawn-task-1070ec](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md#spawn-task-1070ec)

Overview: [000 Overview clusterline getting started](../entry/000%20Overview%20clusterline%20getting%20started.md)

Issues encountered: [002 Issues for Include rust plug post message in clusterline-ui and replace it with logs in test build](../entry/002%20Issues%20for%20Include%20rust%20plug%20post%20message%20in%20clusterline-ui%20and%20replace%20it%20with%20logs%20in%20test%20build.md)

# Decision

1. `syscall` is declared in typescript. It is tested for existence with a try catch for `ReferenceError`. This way we know if we are in the test environment and we're able to vary the meaning of `post_message` accordingly.
1. The UI expects the consumer (rust) to replace a hidden tag in HTML with a JSON string config. This is used to initialize the widget with the data it needs, like the service so that rust remains stateless and the options for the `sb_options_filter_list`:

````html
<head>
	<span id="render_config_json" hidden>REPLACE_RENDER_CONFIG_JSON</span>
</head>
````

# Journal

2026-07-05 Wk 27 Sun - 07:59 +03:00

````ts
document.getElementById('btn_reset').onclick = () => {{
    const resp_promise = syscall('system.invokeFunction', 'clusterline.post_message', ['{MODULE_TOPIC}', 'btn_reset_onclick', '{{' +
        '"service": {service}' +
    '}}']);

    resp_promise.then((resp) => {{
        console.log("btn_reset awaited response: " + resp);
    }})
}};
````

This is how we had javascript exchange messages with the rust plug. Now we want it to work with `clusterline-ui`.

We should be able to simply check if syscall exists under a wrapper, or if not, just stub it with console.log.

 > 
 > **aside** `??` syntax:  https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#nullish-coalescing

https://stackoverflow.com/questions/43335962/purpose-of-declare-keyword-in-typescript

````ts
declare function syscall(name: string, ...args: any[]) : Promise<any>;
````

This should remove the `cannot find Name` typescript error. Even though it's declared, it might still not exist, so we should check.

2026-07-05 Wk 27 Sun - 18:09 +03:00

Spawn [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/entry/002 Issues for Include rust plug post message in clusterline-ui and replace it with logs in test build](../entry/002%20Issues%20for%20Include%20rust%20plug%20post%20message%20in%20clusterline-ui%20and%20replace%20it%20with%20logs%20in%20test%20build.md) ^spawn-entry-d49eda

[002 Issues for Include rust plug post message in clusterline-ui and replace it with logs in test build > syscall any type rejected by eslint no-explicit-any](../entry/002%20Issues%20for%20Include%20rust%20plug%20post%20message%20in%20clusterline-ui%20and%20replace%20it%20with%20logs%20in%20test%20build.md#syscall-any-type-rejected-by-eslint-no-explicit-any)

2026-07-05 Wk 27 Sun - 18:35 +03:00

````ts
export function get_syscall():
	| ((name: string, ...args: unknown[]) => Promise<unknown>)
	| null {
	if (syscall === undefined) {
		return null;
	} else {
		return syscall;
	}
}
````

This results in an exception in the test build:

````
Uncaught (in promise) ReferenceError: syscall is not defined
    get_syscall http://localhost:8002/index.js:147  
    post_message http://localhost:8002/index.js:154  
    main_sb_options_filter_list http://localhost:8002/index.js:393
````

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch

````ts
export function get_syscall():
	| ((name: string, ...args: unknown[]) => Promise<unknown>)
	| null {
    try {
        return syscall;
    } catch (_) {
        return null;
    }
}
````

Now we can use this to know whether we're in a test environment in runtime.

2026-07-05 Wk 27 Sun - 18:42 +03:00

Another thing to resolve about post message is that we would like to know the service that initiated the UI. The rust plug is stateless and we would prefer not to have any global state.

We did this prior by adding it directly via rust-rendering of javascript.

https://www.w3schools.com/TAGS/att_hidden.asp

We could do something like this that is replaced by rust:

````html
<head>
	<span id="render_config_json" hidden>REPLACE_RENDER_CONFIG_JSON</span>
</head>
````

We can also use this to pass the necessary information like the items to be displayed which can only be known by rust precisely at the time of service invocation rather than later by message repsonse.

We could have requires that the functions interfacing with the UI be built during service invocation, but this should be more flexible and not require this. Just pass the needed information right from the start, as we did when we generated the html with exactly the context it needs earlier in rust hardcoded html/script strings.

2026-07-06 Wk 28 Mon - 03:38 +03:00

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse

````ts
public static from_untyped_array(arr: Array<unknown>): Array<SbOption> | DetailedError<SbOptionFromUntypedArrayError> {
````

We're not able to use `arr[i]` here because we get an error `Object is of type 'unknown'.ts(18046)`. Better have it as an `Array<object>`.

No, even with `Array<object>` it complains even though we are checking if the property is defined. That seems to only be permitted for type `any`, which the linters won't let us use.

1. https://stackoverflow.com/questions/49707327/typescript-check-if-property-in-object-in-typesafe-way
1. https://stackoverflow.com/a/49707533/6944447

We can use the string check `'prop' in obj`. Once we are in a branch where this is true, intellisense and typescript will acknowledge that the property now exists.

2026-07-06 Wk 28 Mon - 04:49 +03:00

For some reason `typeof` recognizes `undefined` but not `null`.

But we can do `(elem.opt_hint == null || typeof(elem.opt_hint) === 'string') &&` and now it recognizes `elem.opt_hint` as `(property) opt_hint: string | null | undefined`.

Instead of `from_untyped_array` we will just implement `from_obj` which is more fundamental/simple.

2026-07-06 Wk 28 Mon - 05:38 +03:00

````rust
    let html = include_str!("./inc/index_sb_options_filter_list.html")
        .replace(modal_selector::DEFAULT_RENDER_CONFIG_VALUE, &format!(r#" {{
            \"service\": \"greet\",
            \"items\": [
                {{ 
                    \"name\":           \"Apple!\",
                    \"opt_hint\":       \"Juicy Hint\",
                    \"desc\":           \"Apple Juice is good for you\",
                    \"active_hint\":    true,
                    \"selected\":       false,
                }},

                {{ 
                    \"name\":           \"Rocks...\",
                    \"opt_hint\":       \"Earthy Hint\",
                    \"desc\":           \"Chewing rocks is bad for you\",
                    \"active_hint\":    true,
                    \"selected\":       true,
                }},

                {{ 
                    \"name\":           \"Ice cream...\",
                    \"opt_hint\":       \"Cold Hint\",
                    \"desc\":           \"Chocolate Mint Ice cream for 5 dollars\",
                    \"active_hint\":    false,
                    \"selected\":       false,
                }}
            ]
        }}"#))
    ;
````

This ended up passing with the escapes over to javascript. Need to remove the escapes. We also need to make sure there's no comma at the end of each object.

![Pasted image 20260706054417.png](../../../../../../../attachments/Pasted%20image%2020260706054417.png)

It works! We're able to pass in the options from rust as JSON to javascript which are parsed and validated according to the schema and then used to render the items from the initial effect!

````
[clusterline plug] panicked at src/plug/message_post.rs:20:5: Received unsubsribed message on sb_options_filter_list - onCancel - { service: "greet" }
````

We need to also make sure we subscribe to these test events, but this proves that we're able to receive the messages as well with the correct service tagged.

However, when we cancel, and we use the greet command again to test bring the modal selector back up, it won't work a second time until a refresh.

2026-07-06 Wk 28 Mon - 06:55 +03:00

Proper semantics for creating the UI bundle:

````rust
    let sb_options = inv(sb_options_filter_list::SbOptionVec::new(
        vec![
            SbOption { 
                name: "Apple!".to_owned(), 
                opt_hint: Some("Juicy Hint".to_owned()), 
                desc: "Apple Juice is good for you".to_owned(), 
                active_hint: true, 
                selected: false,
            },

            SbOption { 
                name: "Rocks...".to_owned(), 
                opt_hint: Some("Earthy Hint".to_owned()), 
                desc: "Chewing rocks is bad for you".to_owned(), 
                active_hint: true, 
                selected: true,
            },

            SbOption { 
                name: "Ice cream...".to_owned(), 
                opt_hint: Some("Cold Hint".to_owned()), 
                desc: "Chocolate Mint Ice cream for 5 dollars".to_owned(), 
                active_hint: false, 
                selected: false,
            },

            SbOption { 
                name: "Secret~".to_owned(), 
                opt_hint: None, 
                desc: "".to_owned(), 
                active_hint: false, 
                selected: false,
            },
        ]
    ));

    let ui_bundle = widgets::sb_options_filter_list::new_ui_bundle("greet", sb_options);

    widgets::sb_options_filter_list::show_panel(&ui_bundle).await
````

This shows up alright, and we do respond to the cancel event now:

````
# javascript
[sb_dialog1 > on:cancel]
# rust
[clusterline plug] [rust|on_canceled] You posted a message! { service: "greet" }
[clusterline plug] (service some_service) (event on_canceled)
# javascript
message response for cancel: on_canceled OK
````

We do not yet parse the json message, so rust is reporting `(service some_service)` but we can see that the JSON message with the correct service is there.

[003 Modal loaded to silverbullet as ui bundle fails to deallocate on cancel due to rust future async ignored](../issue/003%20Modal%20loaded%20to%20silverbullet%20as%20ui%20bundle%20fails%20to%20deallocate%20on%20cancel%20due%20to%20rust%20future%20async%20ignored.md)

2026-07-07 Wk 28 Tue - 08:52 +03:00

The simplest solution is to just let the UI close itself. It's one less async operation we have to deal with in rust if we don't have to.

We're able to parse the json message now. Since we're working a lot with js values it is easy:

````rust
let msg = JSON::parse(json_msg)
	.expect("on_selected_json: Failed to parse JSON message");

let service: String = js_value_conv::try_get_jsvalue_prop(&msg, "service")
	.expect("Message must include service");
````

These event handlers are considered application IO facing code, so we do panic if things go wrong here, but we are able to write the UI so that we only emit the expected JSON, so it isn't an issue.

Also silverbullet is generating so much noise with its indexing and other error logs, so let's append to every log message `CLSTR` and filter by it:

````rust
// in /home/lan/src/cloned/cb/lan22h/clusterline-sb/clusterline-rs/src/util/logging.rs
pub fn plug_log(s: &str) {
    imported::log(&format!("CLSTR-RS {s}"));
}
````

````ts
// in /home/lan/src/cloned/cb/lan22h/clusterline-sb/clusterline-ui/src/ts/utils/logging.ts
export function plug_log(s: string) {
	console.log(`CLSTR-UI ${s}`);
}

export function plug_error(s: string) {
	console.error(`CLSTR-UI ${s}`);
}

// in /home/lan/src/cloned/cb/lan22h/clusterline-sb/src/clusterline.ts
export function plug_log(s: string) {
  console.log(`CLSTR-SB ${s}`);
}

export function plug_error(s: string) {
  console.error(`CLSTR-SB ${s}`);
}
````

--/ 2026-07-07 Wk 28 Tue - 09:53 +03:00

Correcting for

````ts
  ℹ Template literals are preferred over string concatenation.

    5 │ export function plug_error(s: string) {
  > 6 │         console.error('CLSTR-UI ' + s);
      │                       ^^^^^^^^^^^^^^^
    7 │ }
    8 │
````

--/ 2026-07-07 Wk 28 Tue - 09:56 +03:00

we also need to watch out for panics, since they wont have this keyword `CLSTR`.

--/

Currently building with

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterline-sb
./build.sh && cp ./dist/lan22h/clusterline-sb/* ~/src/cloned/cb/deltatraced/deltatraced/Library/lan22h/clusterline-sb/
````

Copying over the files to my library in the note repository which are just the `clusterline.plug.js` and `clusterline.plug.yaml`.

2026-07-07 Wk 28 Tue - 17:42 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterline-sb

git commit

# out {
[main af247f1] allow clusterline-ui to build multiple widgets with render config
# }

git log

# out (relevant) {
Author: Mohammed Alzakariya <lanhikarixx@gmail.com>
Date:   Tue Jul 7 17:40:27 2026 +0300

    allow clusterline-ui to build multiple widgets with render config

    - pass render config to clusterline-ui
    - clusterline-ui is able to post messages over to rust plug from
      silverbullet. Becomes just log messages when testing the widget in
      isolation
# }
````

OK
