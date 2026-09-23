---
context_type: task
status: todo
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned in: [^spawn-task-c536a0](../000-clusterline-md-getting-started.md#spawn-task-c536a0)

# Journal

2026-05-22 Wk 21 Fri - 21:15 +03:00

https://v2.silverbullet.md/Plugs/Development

2026-05-22 Wk 21 Fri - 21:47 +03:00

The first command to add is to be able to write the timestamps! `2026-05-22 Wk 21 Fri - 21:47 +03:00`

Getting datetime in ts: https://stackoverflow.com/a/55307111/6944447

In obsidian, we had the date format in `templates` set as:

````
YYYY-MM-DD \W\k \\W ddd
````

and time:

````
HH:mm Z
````

Using `date`:

````sh
date +%Y-%m-%d\ Wk\ %V\ %a\ -\ %R\ %:z
````

Formatting number in style of `{n:02}`: From https://www.spguides.com/typescript-number-format-2-digits/,

````ts
`${n.toString().padStart(2, '0')}`
````

Timezone offset: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTimezoneOffset

2026-05-23 Wk 22 Sat - 00:11 +03:00

Timestamp acquired! Just use `Ctrl+/ tim`. The full command name is `Clusterline: Insert Timestamp`.

2026-05-23 Wk 22 Sat - 00:14 +03:00

Okay so now back onnn track, let’s try to write this plugin in Rust. Right now, it is setup to be written in typescript, and the `.yaml` file is interpreted to read from `hello.ts` somehow.

2026-05-23 Wk 21 Sat - 01:52 +03:00

Spawn [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/001 create a rust silverbullet plugin](001%20create%20a%20rust%20silverbullet%20plugin.md) ^spawn-task-fa5f84
