---
context_type: task
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/005 Add a custom fuzzy selector window with some text](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md)

Spawned in: [^spawn-task-0f48e4](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md#spawn-task-0f48e4)

# Decision

1. Use an environmental variable `$INDEX` to build different `index_*.{ts/html/scss}` files per desired service.

# Journal

2026-07-05 Wk 27 Sun - 05:37 +03:00

The easiest solution would be with a build flag. We only need to build one widget at a time, and get one html/js bundle at a time.

There was some discourse on how far `npm run` wants to support flags:

1. https://stackoverflow.com/questions/76972845/how-do-i-add-conditional-flag-in-my-script-in-package-json-file
1. https://github.com/npm/npm/pull/5518

For simplicity, we can just use environment variables.

````json
// in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-ui/package.json

{
	"scripts": {
		"greet": "echo Hello $SomeUser"
	}
}
````

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-ui
SomeUser="Lan" npm run greet

# out
> clusterline-ui@1.0.0 greet
> echo Hello $SomeUser

Hello Lan
````

We have `posthtml` for html building and `esbuild` for javascript building. Both would still need to accept this as a compilation option.

So this turns out to not be so simple. Here is a hack: Create multiple versions of `index.html` and `index.ts`, per widget, under `./src/html/index/` and `./src/ts/index/`. Then in `/home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-ui/build.sh` we route the desired `index.*` via an environmental variable. In our case we want all of them. This is sort of analogous to different `bins` in rust, each being its own entry, although put together with copying around.

We will make a `select_index.sh` file to make this operation formal and document that it needs to be run before a build.

2026-07-05 Wk 27 Sun - 06:44 +03:00

The `index/` extra folder messes with the local imports. Instead let it be `index_{INDEX}.html` or `index_{INDEX}.ts` which is specified by the environmental variable `INDEX`.

Also make sure to gitignore `index.ts` and `index.html` since they are now build artifacts.

2026-07-05 Wk 27 Sun - 06:48 +03:00

Let's try to just not have an `index.html` and `index.ts` in `src/`. Let them all be `index_{INDEX}`. Then via environment variable to `npm run build`, route it. This way we do not have unnecessary build artifacts in `src/`.

`npx posthtml` doesn't care that we are using `index_{INDEX}.html`, it will simply generate the mirror name under `public/` so we can make use of this. When buiilding html, just move `./public/index_{INDEX}.html` to `./public/index.html`.

`esbuild` takes the `index.ts` file directly, so it is now passed via environment variable `$INDEX`.

Reporting in documentation the list of valid values for `$INDEX` is `ls ./src/html/index_*.html | cut -d'_' -f2- | cut -d'.' -f1`.

We need to do the same for `scss`. `postcss` also just posts a `index_{INDEX}.css` and `index_{INDEX}.css.map` that we can move into `index.css` and `index.css.map`.

We also need to remove all `index_*.css`. and same for `index_*.html` to avoid cluttering the other binaries which build simultaneously but with no valid javascript.

Now we are able to create the new widget `text_input_form`, which we will need in addition to the `sb_options_filter_list` widget.

Updated `clusterline-ui/build.sh` to save `index_{INDEX}.html` and `index_{INDEX}.js` for the rust plug project to make use of the current html/js bundle it needs.

We confirm that app switching via `$INDEX` works all the way through being passed to silverbullet by the rust plug.

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterlinemd
git commit
[main e63b9ce] allow clusterline-ui to build multiple widgets
````

OK
