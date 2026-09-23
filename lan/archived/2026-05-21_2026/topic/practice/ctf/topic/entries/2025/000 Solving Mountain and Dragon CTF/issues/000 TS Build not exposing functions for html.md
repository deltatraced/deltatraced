# 1 Journal

* [x] 

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

`--bundle --format=iife` puts everything inside a block, so `mainloop` is not accessible to the html file.

`--bundle --format=cjs` generates extra CommonJS code that's unwanted (including module.exports), although it does expose the functions. This is node specific as the `--help` clarifies as well.

2025-07-30 Wk 31 Wed - 08:15

We could just remove the invocations.

````js
(() => {
[...]
 })();
 
````

Remove those lines for `--format=iife`.

````sh
cat build/$basename.js | python3 -c "import sys; lines = sys.stdin.read().split('\n'); print('\n'.join(lines[1:-2]))" > build/tmp
cat build/tmp > build/$basename.js
rm build/tmp
````

Not a pretty workaround with the tmp file, but we can't cat and then file write simultaneously here.
