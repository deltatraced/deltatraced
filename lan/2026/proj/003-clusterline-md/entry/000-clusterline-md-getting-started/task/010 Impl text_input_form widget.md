---
context_type: task
status: todo
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned in: [^spawn-task-fe35de](../000-clusterline-md-getting-started.md#spawn-task-fe35de)

# Journal

2026-07-11 Wk 28 Sat - 01:19 +03:00

Just like `sb_options_filter_list` implemented through [005 Add a custom fuzzy selector window with some text](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md) we want another basic widget: The text input form widget:

````
___________________________________
| {**Bold H1 Title**}              |
| {Label1}                 {Input1}|
| {Label2}                 {Input2}|
|__________________________________|
|__________________________[submit]|

````

Pretty similar to how obsidian does it.  Takes as render config an array of labels. Emits messages for `cancel` and `confirm` events. Both include the service. `confirm` also includes an array of values in the order of the labels. The consumer does respond to `confirm` with validation. If OK, the widget drops, otherwise it will display an error message and let the user try again.

2026-07-11 Wk 28 Sat - 22:30 +03:00

Doing some refactoring to clarify in caller site what functions log on error so we don't also throw a duplicate exception assuming it does not. This side effect (and all side effects) needs to also be documented in the function docs.

Actually it should be better to only document it on the function body. We shouldn't expect the consumer to litter their code with disclaimers about the called code. We should check that to discern whether to silently return or throw an exception on invariant violations. In the docs, we're also putting all side effect descriptions in bullets under `### Side Effects`.

We need to also consider our prior policy decision [000 InternalErrors must not be exposed to module consumers](../judgment/000%20InternalErrors%20must%20not%20be%20exposed%20to%20module%20consumers.md). So again the point is not exhaustive description of side effects. If a side effect occurs only on an invariant violation event, it must not be reported to module consumers.
