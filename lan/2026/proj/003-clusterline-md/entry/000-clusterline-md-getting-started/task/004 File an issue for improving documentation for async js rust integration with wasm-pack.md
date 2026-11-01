---
context_type: task
status: pend
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/001 create a rust silverbullet plugin](001%20create%20a%20rust%20silverbullet%20plugin.md)

Spawned in: [^spawn-task-5426a0](001%20create%20a%20rust%20silverbullet%20plugin.md#spawn-task-5426a0)

# Journal

2026-06-04 Wk 23 Thu - 07:29 +03:00

The documentation in https://wasm-bindgen.github.io/wasm-bindgen/reference/types/js-sys.html#promiset

shows how we really should write our function declarations to have async integration between rs and js.

https://wasm-bindgen.github.io/wasm-bindgen/api/js_sys/futures/index.html mentions backward compatibility, but it provides genuinely different semantics. An extern declaration returning `Promise<T>` yields a result on `.await`, unlike marking the extern declaration `async`. If backwards compatibility in my context is taken to suggest they behave identically; then this is mistaken.

And this is the directly misleading section, at least when interpreted in light with how it can be used alongside wasm-pack: https://wasm-bindgen.github.io/wasm-bindgen/reference/js-promises-and-rust-futures.html#importing-js-async-functions

They recommend to use async, this could have a note about using `Promise<T>`

https://github.com/wasm-bindgen/wasm-bindgen/blob/main/guide/src/reference/js-promises-and-rust-futures.md#importing-js-async-functions

In https://github.com/wasm-bindgen/wasm-bindgen/issues/4634,

they seem to want to possibly phase out of wasm-pack, but they still use it.

I filed the issue:

https://github.com/wasm-bindgen/wasm-bindgen/issues/5182

Track more about the issue in [000 Issue 5182 wasm-bindgen](../../../../../topic/oss/000%20OSS%20Contrib/issue/000%20Issue%205182%20wasm-bindgen/000%20Issue%205182%20wasm-bindgen.md)
