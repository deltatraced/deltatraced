# 1 Journal

* [x] 

2025-07-30 Wk 31 Wed - 04:26

The html and javascript used here seems self-contained. We can enhance our tools if we can modify the source code to play around with it and annotate it as we go.

To test that modification to the script is recognized, we will temporarily change the black spot input responses to all Ts:

````js
  mq.innerHTML = " ";
  mq.innerHTML += (inp >> 0) & (1 == 1) ? "T" : " ";
  mq.innerHTML += (inp >> 1) & (1 == 1) ? "T" : " ";
  mq.innerHTML += (inp >> 2) & (1 == 1) ? "T" : " ";
  mq.innerHTML += (inp >> 3) & (1 == 1) ? "T" : " ";
  mq.innerHTML += (inp >> 4) & (1 == 1) ? "T" : " ";
  mq.innerHTML += (inp >> 5) & (1 == 1) ? "T" : " ";
  mq.innerHTML += " ";
````

2025-07-30 Wk 31 Wed - 04:33

This works. Our javascript responses are recognized. Reverting back to

````js
  mq.innerHTML = " ";
  mq.innerHTML += (inp >> 0) & (1 == 1) ? "L" : " ";
  mq.innerHTML += (inp >> 1) & (1 == 1) ? "R" : " ";
  mq.innerHTML += (inp >> 2) & (1 == 1) ? "F" : " ";
  mq.innerHTML += (inp >> 3) & (1 == 1) ? "B" : " ";
  mq.innerHTML += (inp >> 4) & (1 == 1) ? "I" : " ";
  mq.innerHTML += (inp >> 5) & (1 == 1) ? "U" : " ";
  mq.innerHTML += " ";
````

Now we're going to modify `adventure.html` to point to our new js script that is `build/mountdrag.js`. We will maintain `mountaindrag.js` as the original copy.

We will also upgrade to `mountdrag.ts` because we can. Adding a `build.sh` that uses similar logic to this [entry](https://github.com/LanHikari22/lan-setup-notes/blob/3fbc4bad0a8e4f49739119f1acd88bf23f039bfb/lan/topics/tooling/web/entries/latest/000%20Making%20Greasemonkey%20scripts.md#16-creating-command-code-that-runs-every-t-ms).

````sh
#!/bin/bash

script_dir=$(dirname "$(readlink -f "$0")")

build() {
    basename="$1"

    npx esbuild "$basename.ts" --bundle --format=iife --outfile=build/$basename.js
}

pushd $script_dir

build mountdrag

popd
````

Oops there are many errors in the ts file if copied as is. See this [task](002%20Run%20the%20game%20local%20with%20ability%20to%20modify%20the%20underlying%20script.md#32-investigate-fixing-typescript-errors-from-copies-mountdragjs).
