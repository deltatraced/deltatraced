# Journal

2026-06-13 Wk 24 Sat - 08:56 +03:00

https://github.com/wasm-bindgen/wasm-bindgen/issues/5182#issuecomment-4694258106

It seems they are unable to reproduce the issue I submitted with async. This originated from [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/004 File an issue for improving documentation for async js rust integration with wasm-pack](../../../../../proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/004%20File%20an%20issue%20for%20improving%20documentation%20for%20async%20js%20rust%20integration%20with%20wasm-pack.md)

We need to provide a full replication of this to pass on, and to also verify it ourselves using a new docker container system before doing so.

Let's move deltachives to codeberg and create a repro to share with them. Let's just call the organization `lan22h-experiments` for maximum clarity.

https://codeberg.org/lan22h-experiments/repro-wasm-bindgen-5182

We need some minimal `ts -> js`  project. We can use `~/src/cloned/gh/LanHikari22/lan-exp-scripts/2025/files/persistent/000-mountain-n-dragon-ctf/build.sh`.

https://html.spec.whatwg.org/, https://valex.github.io/script_tag.html

2026-06-13 Wk 24 Sat - 12:34 +03:00

We still reproduce the problem with `20fea18`: https://codeberg.org/lan22h-experiments/repro-wasm-bindgen-5182/commit/20fea18a904d1ec16ed1bf0578e6174eacef987a

2026-06-13 Wk 24 Sat - 12:46 +03:00

Now let's make sure to build this on a clean environment, and add docker container support to the repro.

Using similar setup to what I had in `~/src/cloned/gh/deltatraced/delta-box/box/box000_blank_system/` and https://github.com/LanHikari22/dbmint.

2026-06-13 Wk 24 Sat - 17:11 +03:00

So we have the system building in docker, but the issue is that it takes a long time (some recorded at 2 minutes) when running the build, due to this step through `wasm-pack build --target web`:

````
[INFO]: Installing wasm-bindgen...
````

2026-06-13 Wk 24 Sat - 19:54 +03:00

Hmm. Seems faster now that I have it run non-root, I think.

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/repro-wasm-bindgen-5182
git commit -m "added docker support with ubuntu image"

# out
[main 40e6fcf] added docker support with ubuntu i
````

Now let's try to reproduce this under alpine instead of ubuntu: https://www.alpinelinux.org/, https://hub.docker.com/\_/alpine/

2026-06-13 Wk 24 Sat - 21:05 +03:00

This seems a bit involved. there are differences between systems with how they treat the rustup installation, and possibly user configuration, and we need some of those to be installed local.

But for now the ubuntu image should be good for the repro.

Submitted repro repo in the issue.
