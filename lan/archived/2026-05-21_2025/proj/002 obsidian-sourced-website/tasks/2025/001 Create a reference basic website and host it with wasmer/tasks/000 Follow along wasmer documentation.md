# 1 Journal

* [ ] 

From [^spawn-task-229769](000%20Follow%20along%20wasmer%20documentation.md#spawn-task-229769) in [6.1 Investigate Wasmer deployment](000%20Follow%20along%20wasmer%20documentation.md#61-investigate-wasmer-deployment)

2025-08-26 Wk 35 Tue - 21:32

Following with [wasmer docs](https://docs.wasmer.io/).

Already installed this, but need to update

````sh
wasmer self-update
````

2025-08-26 Wk 35 Tue - 22:08

In [wasmer docs CLI](https://docs.wasmer.io/runtime/cli) they mention a `wasmer.toml` file that allows the folder to be recognized as a local package and be run with `wasmer run .` Looking at my [starter astro website](https://github.com/LanHikari22/astro-starter-website), we also have a `wasmer.toml`:

````toml
[dependencies]
"wasmer/static-web-server" = "*"

[fs]
public = "dist"
````

2025-08-26 Wk 35 Tue - 22:49

In [wasmer docs wasix](https://docs.wasmer.io/runtime/runners/wasix),

 > 
 > WASIX was created by the Wasmer team to speed up the Wasmification of codebases around the world!

I guess Wasmification is a word!

We do want to try to make CGI executables, so [WCGI](https://docs.wasmer.io/runtime/runners/wcgi) is worth looking at.

2025-08-26 Wk 35 Tue - 22:53

In [registery getting-started](https://docs.wasmer.io/registry/get-started) they share the [wasmer.toml manifest](https://docs.wasmer.io/registry/manifest).

Spawn [7.1 wasmer.toml as reference for toml file documentation](000%20Follow%20along%20wasmer%20documentation.md#71-wasmertoml-as-reference-for-toml-file-documentation) ^spawn-idea-24ee1a

2025-08-26 Wk 35 Tue - 23:10

In [build your package](https://docs.wasmer.io/registry/get-started#build-your-package) they show an example of creating a `*.wasm` file with Rust using their WASIX runner and also WASI.

2025-08-26 Wk 35 Tue - 23:19

In [wasmer architecture](https://docs.wasmer.io/edge/architecture),

So each node in the vertical stack uses a monolith capable of running an application itself. They explain that this means it's [vertically integrated](https://en.wikipedia.org/wiki/Vertical_integration).

It also uses a [shared-nothing architecture](https://en.wikipedia.org/wiki/Shared-nothing_architecture), so the nodes themselves will be able to continue to operate even if all others nodes are offline because they have no shared state that would cause failure.

2025-08-26 Wk 35 Tue - 23:41

Statelessness, CGI scripts as [idompotent](https://en.wikipedia.org/wiki/Idempotence) functions... Many cool functional principles here.

I'm wonder if by each node being stateless it would also mean that they are always able to fulfill their function and not store state themselves. Some state should exist in the broader application though. But by the shared nothing principle, no state will be accessible by all nodes. Where would the database go?

Spawn [6.2 Stateless nodes that share nothing but where does the database go and how is it integrated?](000%20Follow%20along%20wasmer%20documentation.md#62-stateless-nodes-that-share-nothing-but-where-does-the-database-go-and-how-is-it-integrated) ^spawn-invst-f3bfe8
