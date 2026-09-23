---
context_type: task
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/005 Add a custom fuzzy selector window with some text](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md)

Spawned in: [^spawn-task-3c7bf1](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md#spawn-task-3c7bf1)

Overview: [000 Overview clusterline getting started](../entry/000%20Overview%20clusterline%20getting%20started.md)

# Journal

2026-07-07 Wk 28 Tue - 21:04 +03:00

We implemented confirmed selection and now post an `on_selected` message besides `on_canceled`.

There are some events that are not triggering:

* `keyDown` and `keyUp` for `sb_input1`.
* `mouseMove` for one of the option items.

https://www.w3schools.com/tags/ref_eventattributes.asp

2026-07-10 Wk 28 Fri - 09:59 +03:00

Just had to change them to `keydown`, `keyup`, and `mousemove`.

2026-07-10 Wk 28 Fri - 12:32 +03:00

Okay, right now we set `keydown` to handle moving up and down. We're doing like in silverbullet, registered for the case when the input text is focused. We also have basic filtering functionality now. Keyword based (space separated list of words to include/disclude). If the keyword starts with `!` it will be excluded. This is how I've done this in fzf mostly and with setting it in vim. Although they have that be fuzzy, so I usually try to opt out of the fuzzying by using `'`. But here no need for `'`. And because it is keyboard based, order of the keywords also doesn't matter. Obsidian often is sensitive in its search for the text order for some reason.

Search includes the shared text of the name, hint, and description. It is also case insensitive.

Not currently using `keyup`.  Silverbullet (`client/components/filter.tsx > fn FilterList > fn onKeyUp`) seems to use it for some alt+space function.

Pressing `Enter` now also selects, so the user doesn't have to use the mouse.

But we need to implement `mousemove` next for users who prefer to move the mouse and click.

`mousemove` set! Just have to select whichever item is currently selected!

--/ 2026-07-10 Wk 28 Fri - 20:25 +03:00

Need to also handle case where everything is filtered. Nothing should be selected. And if the user presses Enter, it shouldn't confirm anything.

--/--/ 2026-07-11 Wk 28 Sat - 01:12 +03:00 | Enter on an empty filter now no longer confirms. We also reset selection in that case.
--/--/

--/

2026-07-10 Wk 28 Fri - 13:21 +03:00

The new interactivity works in silverbullet as well, but we're not receiving the confirm event in rust. We do receive the cancel event in rust.

There was a panic I did not see from rust because I was filtering by `CLSTR`. Change that filter to just `clusterline` so we can see panics too. (Although we pay for this with currently occasional minimal noise from the index plugin on the clusterline plugin in the repo)

````
[clusterline plug] [clusterline-rs] We do reach on_selected_json
[clusterline plug] panicked at src/widgets/sb_options_filter_list.rs:102:14: on_selected_json: Failed to parse JSON message: JsValue(SyntaxError: JSON.parse: expected ',' or '}' after property value in object at line 3 column 6 of the JSON data
````

Forgot to add comma to the JSON message.

2026-07-10 Wk 28 Fri - 21:16 +03:00

````
[clusterline plug] [clusterline-sb] Plug Error: post_message expects arguments topic, subtopic, json_msg
````

Refining the error under `src/clusterline.ts > fn ts_post_message`.

````
[clusterline plug] [clusterline-sb] Plug Error: post_message expects arguments topic, subtopic, json_msg. We got (comma_separated_args sb_options_filter_list,on_selected,{ "service": "greet", "option_name": "Rocks..." }).
````

Right. Comma separation broke because the json message itself has a comma now. It is fine to use the comma separator, but let's stop at the first 3. The topic and subtopic should not themselves have commas, the remaining is all message.

````ts
// in src/clusterline.ts > fn ts_post_message
  const topic = tokens[0];
  const subtopic = tokens[1];
  const json_msg = comma_separated_args.replace(topic + ",", "").replace(subtopic + ",", "");
````

--/ 2026-07-10 Wk 28 Fri - 22:45 +03:00 | `comma_separated_args` is not guaranteed to be a string: need to cast to string explicitly.
--/

We're now able to spawn the widget with consumer-determined options in rust, it is basically interactable, and it when the user confirms, we get the output back in rust:

````
[clusterline plug] [clusterline-rs] (service greet) (event on_selected) (line Ice cream...)
````

2026-07-11 Wk 28 Sat - 01:17 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterline-sb
git commit

# out
[main c51379f] impl sb_options_filter_list interactivity
````

OK

2026-07-11 Wk 28 Sat - 03:20 +03:00

There's an issue. We need to cycle when the user pressed up or down too much. Trying to implement this, we encountered that we do not track the shown list indices, but only the full indices currently. We need to get the prior and next indices as currently shown.

2026-07-11 Wk 28 Sat - 07:58 +03:00

We violate an invariant when typing and then pressing down:

````
Uncaught DOMException: The index must exist during search
````

This is an issue:

````ts
// in /home/lan/src/cloned/cb/lan22h/clusterline-sb/clusterline-ui/src/ts/components/sb_options_list_component.ts > fn SbOptionsListComponent::set_filter
// Reset back to selecting the first item or select nothing
if (mut_num_shown !== 0) {
	this.set_selected(0, true);
} else {
	this.reset_selected();
}
````

Selected should be the first *filtered* item, not the first item according to the full list. So this is not guaranteed to exist in our filter, violating our invariant. This is the case with my test case with filter `2`, item number 2 is not the first item in the full list. the first item has been filtered out, yet was selected by this.

We need to have the filtered list be part of our state. Saving `num_shown` is just not enough, this is a projection off of the new list that we must work with. If no filter is applied, it should be identical to the full list. And we also need `set_filtered_selected` to work with the filtered indices instead. Document the distinction for both.

2026-07-11 Wk 28 Sat - 08:22 +03:00

Another thing, when there are no results, `ArrowUp` and `ArrowDown` instead move the character cursor in the input box. We don't want this. Actually, it's active even when there are results. That capability is just not disabled.

Though we do want it to partially work, for left and right if the wants to move the cursor, just not up and down.

Adding `e.preventDefault()` to `keydown` of `sb_input1` prevents all these capabilities. We can't use the arrows on the input, and we can't even type or erase for that matter, which is too much.

Let's do `e.preventDefault()` not unconditionally, but only in codes `ArrowUp` and `ArrowDown`: It works! everything else works now.

Confirmed working in silverbullet.

2026-07-11 Wk 28 Sat - 21:46 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterline-sb
git commit

# out
[main 1255488] fix sb_options_filter_list filtered list navigation and duplicate updown arrow keys
````

OK
