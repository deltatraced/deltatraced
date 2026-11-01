# 1 Journal

* [ ] 

2025-09-02 Wk 36 Tue - 13:05

We are able to deploy two different projects now, but they do not share the same domain name. We have a single domain name, and would like to dedicate different pages to different npm projects. This tutorial might help.

Tutorial is [here](https://turbocloud.dev/book/deploying-node.js-under-one-domain-with-caddy/).

2025-09-02 Wk 36 Tue - 13:07

Let's make a temporary tutorial project.

````sh
mkdir -p ~/src/tmp/del/tut
````

2025-09-02 Wk 36 Tue - 13:44

They use [Caddy](https://caddyserver.com/).

We might need some integration to use this with wasmer. There is [valpackett/caddy-wasm-wcgi](https://codeberg.org/valpackett/caddy-wasm-wcgi)

2025-09-02 Wk 36 Tue - 14:56

I asked on wasmer discord, and the founder said that it's not an available feature yet, but it is possible yet with 3 apps but it is possible with a custom router in a single app?

2025-09-02 Wk 36 Tue - 16:21

You can also find some of the language projects implemented under WASIX [here](https://github.com/wasix-org).

2025-09-02 Wk 36 Tue - 16:38

In [cowsay wasmer.toml](https://github.com/wapm-packages/cowsay/blob/master/wasmer.toml) they are feeding a `cowsay.wasm` executable directly, but this is for a command rather than a service.

2025-09-02 Wk 36 Tue - 16:55

Spawn [4.2 Open an issue to Wasmer docs for broken links](004%20Find%20and%20follow%20process%20to%20deploy%20multiple%20npm%20projects%20under%20wasmer.md#42-open-an-issue-to-wasmer-docs-for-broken-links) ^spawn-issue-fe8e6b

2025-09-02 Wk 36 Tue - 17:18

It's pretty streamlined on rust: [wasmer.io rust-wcgi](https://wasmer.io/templates/rust-wcgi?intent=at_ynm3Iet1Cr2Z). Just gotta add the right build target!

### 1.1.1 Pend
