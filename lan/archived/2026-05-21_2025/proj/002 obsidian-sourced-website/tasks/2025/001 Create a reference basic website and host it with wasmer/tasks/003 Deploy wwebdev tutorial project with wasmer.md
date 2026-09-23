# 1 Journal

* [x] 

From [^spawn-task-7ad6e5](003%20Deploy%20wwebdev%20tutorial%20project%20with%20wasmer.md#spawn-task-7ad6e5) in [3.3 Follow with wweb static npm website tutorial](003%20Deploy%20wwebdev%20tutorial%20project%20with%20wasmer.md#33-follow-with-wweb-static-npm-website-tutorial)

2025-09-02 Wk 36 Tue - 11:53

We can use wasmer's [static website example](https://github.com/wasmer-examples/static-website) on github.

This uses their [WASIX](https://docs.wasmer.io/runtime/runners/wasix) runner over `wasmer-static-web-site`. Let's copy their [wasmer.toml](https://raw.githubusercontent.com/wasmer-examples/static-website/refs/heads/main/wasmer.toml):

(update)

````toml
[dependencies]
"wasmer/static-web-server" = "^1"

[fs]
"/public" = "public"
"/settings" = "settings"

[[command]]
name = "script"
module = "wasmer/static-web-server:webserver"
runner = "https://webc.org/runner/wasi"

[command.annotations.wasi]
main-args = ["-w", "/settings/config.toml"]
````

<details>
<summary>errata</summary>

2025-09-02 Wk 36 Tue - 12:15
Seems it requires us to use `/public`:

````sh
2025-09-02T09:10:09.424496Z ERROR static_web_server::server: server failed to start up: root directory was not found or inaccessible

Caused by:

    path /public was not found or inaccessible
````

Move everything `/dist` to `/public`.

Then

````sh
wasmer app delete
wasmer app create # and deploy
````

</details>

(/update)

Copy their [settings/config.toml](https://raw.githubusercontent.com/wasmer-examples/static-website/refs/heads/main/settings/config.toml).

Copy [app.yaml](https://raw.githubusercontent.com/wasmer-examples/static-website/refs/heads/main/app.yaml).

Then, we can deploy with

````sh
wasmer deploy
````

2025-09-02 Wk 36 Tue - 12:19

And it works!
