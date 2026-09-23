---
context_type: task
status: done
---

Parent: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/000 Getting started ping-pong-score-ts](../000%20Getting%20started%20ping-pong-score-ts.md)

Spawned by: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/000 Getting started ping-pong-score-ts](../000%20Getting%20started%20ping-pong-score-ts.md)

Spawned in: [^spawn-task-34a9db](../000%20Getting%20started%20ping-pong-score-ts.md#spawn-task-34a9db)

# Journal

2026-06-22 Wk 26 Mon - 09:58 +03:00

[so ans How to check TypeScript code for syntax errors from a command line?](https://stackoverflow.com/a/61794106/6944447)

We need an `eslint.config.js` for `eslint` to run: https://eslint.org/docs/latest/use/configure/

1. https://eslint.org/docs/latest/use/configure/rules
1. https://eslint.org/docs/latest/rules/

We're adding an `eslint.config.ts` instead:

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
npm i -d eslint
npx --node-options='--experimental-strip-types' eslint --flag unstable_native_nodejs_ts_config
````

says requires jitti despite this alternative config.

````sh
npm install --save-dev jiti
````

https://www.npmjs.com/package/jiti

Still complains that I need to install it.

````
Error: The 'jiti' library is required for loading TypeScript configuration files. Make sure to install it.
````

````
npm ls jiti
ping-pong-score-ts@1.0.0 /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
├─┬ eslint@10.5.0
│ └── jiti@2.7.0 deduped
├── jiti@2.7.0
└─┬ postcss-cli@11.0.1
  └─┬ postcss-load-config@5.1.0
    └── jiti@2.7.0 deduped
````

````
npm uninstall -g eslint
npm uninstall -g jiti
npm install -g jiti
npm install -g eslint
````

This does advance the problem.

````
ESLint: 10.5.0

Error: Cannot find module 'eslint/config'
````

2026-06-24 Wk 26 Wed - 02:10 +03:00

https://typescript-eslint.io/getting-started/

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
npm install --save-dev eslint @eslint/js typescript typescript-eslint
````

Set up the extending to recommended configs, and now we can use `npx eslint .` in the project dir. But since we're only typechecking `.ts` and generating `.js`, replace their config with `**/.ts` for files:

````js
files: ["**/*.ts"],
````

But we still get no alarm for

````ts
function get_int(): number {
  return "nope";
}
````

2026-06-24 Wk 26 Wed - 02:23 +03:00

Seems somewhere I expected eslint to do what `tsc` does as well, but this seems to be mistaken, as informed by https://stackoverflow.com/a/78743926/6944447.

Let's also use `tsc`.

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
npm install --save-dev tsc
````

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
npx tsc src/ts/index.ts

# out
src/ts/index.ts:16:3 - error TS2322: Type 'string' is not assignable to type 'number'.

16   return "nope";
     ~~~~~~


Found 1 error in src/ts/index.ts:16
````

2026-06-24 Wk 26 Wed - 02:32 +03:00

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
npm run check:ts

# out

> ping-pong-score-ts@1.0.0 check:ts
> npx tsc ./src/ts/index.ts && eslint .

src/ts/index.ts:2:25 - error TS6142: Module './components/score_table.tsx' was resolved to '/home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts/src/ts/components/score_table.tsx', but '--jsx' is not set.

2 import { get_int } from "./components/score_table.tsx";
                          ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Found 1 error in src/ts/index.ts:2
````

https://github.com/dvictor/jsx-no-react

https://www.webdevtutor.net/blog/typescript-jsx-without-react suggests a `tsconfig.json` with

````
{
	"compilerOptions": {
		"jsx": "preserve"
	}
}
````

Once including this, we need to just invoke `tsc` or it will error about giving it files.

Also many of these:

````ts
src/ts/components/score_row.tsx:21:4 - error TS7026: JSX element implicitly has type 'any' because no interface 'JSX.IntrinsicElements' exists.

21    <table id={this.component_id}>
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

src/ts/components/score_row.tsx:22:5 - error TS7026: JSX element implicitly has type 'any' because no interface 'JSX.IntrinsicElements' exists.

22     <td>{this.left_score}</td>
       ~~~~
````

````ts
src/ts/index.ts:2:25 - error TS5097: An import path can only end with a '.tsx' extension when 'allowImportingTsExtensions' is enabled.

2 import { get_int } from "./components/score_table.tsx";
````

Included

````diff
{
	"compilerOptions": {
		"jsx": "preserve",
+    "allowImportingTsExtensions": true,
+    "noEmit": true
	}
}
````

to get over the `allowImportingTsExtensions` error.

Seems like this is more fragile and not officially supported. Let's look for a different way to render DOM elements in typescript.

https://stackoverflow.com/questions/30430982/can-i-use-jsx-without-react-to-inline-html-in-script $\to$ https://hyperscript.org/docs/getting-started/

Too involved, an entire new language.

https://vanjs.org/, https://nanojsx.io/ (for ssr), https://lemonadejs.com/

Let's try lemonadeJS, seems lightweight, and we want a way to create lightweight reusable components somewhat react-like.

````sh
npm install -d lemonadejs@5
````

Some typescript error from just importing lemonade from lemonadejs, and `render => ...` at first glance doesn't seem to have a clear type or have it be specified.

We can try https://github.com/huyng12/reactive to see if the typescript support is better. Make sure to remove lemonadejs from `package.json`.

````sh
npm install @oddx/reactive
````

Fails with 404.

So no good with this either; it's 6 years out of date.

For now, let's just do typescript.

2026-06-26 Wk 26 Fri - 15:49 +03:00

Since we have a VPS now and can serve the website, we can without the single `index.html` requirement.

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
git commit

# out
[main 82559b4] update build infra
````
