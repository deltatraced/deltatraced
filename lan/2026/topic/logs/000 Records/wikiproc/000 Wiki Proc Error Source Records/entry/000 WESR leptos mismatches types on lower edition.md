---
context_type: entry
---

Parent: [lan/2026/topic/logs/000 Records/wikiproc/000 Wiki Proc Error Source Records/000 Wiki Proc Error Source Records](../000%20Wiki%20Proc%20Error%20Source%20Records.md)

Spawned by: [lan/2026/topic/logs/000 Records/wikiproc/000 Wiki Proc Error Source Records/000 Wiki Proc Error Source Records](../000%20Wiki%20Proc%20Error%20Source%20Records.md)

Spawned in: [^spawn-entry-8199af](../000%20Wiki%20Proc%20Error%20Source%20Records.md#spawn-entry-8199af)

---

# Message

````rust
   Compiling leptos v0.8.19
error[E0308]: mismatched types
   --> /home/lan/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/leptos-0.8.19/src/hydration/mod.rs:178:5
    |
178 | /     view! {
179 | |         <link rel="modulepreload" href=format!("{root}/{pkg_path}/{js_file_name}.js") crossorigin=nonce.clone()/>
180 | |         <link
181 | |             rel="preload"
...   |
189 | |         </script>
190 | |     }
    | |_____^ expected a tuple with 3 elements, found one with 5 elements
    |
    = note: expected struct `tachys::html::element::HtmlElement<_, (Attr<Rel, &str>, Attr<Href, std::string::String>, Attr<Crossorigin, std::option::Option<std::string::String>>), _>`
               found struct `tachys::html::element::HtmlElement<_, (Attr<Rel, &str>, Attr<Href, std::string::String>, Attr<As, &str>, Attr<tachys::html::attribute::Type, &str>, Attr<Crossorigin, std::string::String>), _>`
    = note: this error originates in the macro `view` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0308`.
error: could not compile `leptos` (lib) due to 1 previous error
````

# Versions

````yaml
# in Cargo.toml
[dependencies]
leptos = { version = "0.8.0" }
leptos_router = { version = "0.8.0" }
axum = { version = "0.8.0", optional = true }
console_error_panic_hook = { version = "0.1", optional = true }
leptos_axum = { version = "0.8.0", optional = true }
leptos_meta = { version = "0.8.0" }
tokio = { version = "1", features = ["rt-multi-thread"], optional = true }
wasm-bindgen = { version = "0.2.106", optional = true }
````

# Description

You get this error message if you have the edition in `Cargo.toml` as `2018`.

# Reproduction

From https://github.com/leptos-rs/leptos,

Just follow the instructions

````sh
cargo install cargo-leptos --locked
cargo leptos new --git https://github.com/leptos-rs/start-axum
cd [your project name]
````

But then in `Cargo.toml`, change the edition from `2021` to `2018`

# Solution

Ensure you have the `Cargo.toml` edition as `2021`.

# Cause

Not investigated.
