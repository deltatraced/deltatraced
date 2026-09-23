# 1 Journal

* [ ] 

2025-08-26 Wk 35 Tue - 22:16

Right now in `/home/lan/src/unpub/gh/LanHikari22/basic-ref-website-wasmer`.

````sh
git init
git branch -m master main
cp ~/src/cloned/gh/LanHikari22/dbmint/README.md .
cp ~/src/cloned/gh/LanHikari22/dbmint/LICENSE .
wasmer init
````

Make edits to the README for this reference website.

We're just gonna have `backend`, `frontend`, and `backend/db` for now.

````sh
touch frontend/index.html
````

2025-08-26 Wk 35 Tue - 23:12

Running `wasmer init` basically creates this `wasmer.toml` in my case:

````toml
[package]
name = "lanhikari22/basic-ref-website-wasmer"
version = "0.1.0"
description = "Description for package basic-ref-website-wasmer"

# See more keys and definitions at https://docs.wasmer.io/registry/manifest

[[module]]
name = "basic-ref-website-wasmer"
source = "basic-ref-website-wasmer.wasm"
abi = "wasi"

[module.interfaces]
wasi = "0.1.0-unstable"

[[command]]
name = "basic-ref-website-wasmer"
module = "basic-ref-website-wasmer"
runner = "wasi"
````
