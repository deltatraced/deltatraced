# 1 Journal

* [ ] 

2025-09-01 Wk 36 Mon - 21:08

The tutorial is [here](https://wweb.dev/blog/how-to-create-static-website-npm-scripts).

There is a finished template in [gh wwebdev/static-website-template](https://github.com/wwebdev/static-website-template).

````sh
# in /tmp/del/tut

npm version

# out
11.3.0
````

From `npm init` we get:

````
npm notice
npm notice New minor version of npm available! 11.3.0 -> 11.5.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.5.2
npm notice To update run: npm install -g npm@11.5.2
npm notice
````

````sh
npm install -g npm@11.5.2
````

````sh
# in /tmp/del/tut

npm init
````

This gives us some choices to go through and creates a `package.json`. The schema documentation is [here](https://docs.npmjs.com/cli/v10/configuring-npm/package-json?v=true). But I can't find `type` there...

[stackoverflow answer](https://stackoverflow.com/a/62554884/6944447) $\to$ [nodejs docs esm_enabling](https://nodejs.org/docs/latest-v13.x/api/esm.html#esm_enabling)

2025-09-01 Wk 36 Mon - 22:15

````json
// in /tmp/del/tut
cat package.json

// out
{
  "name": "static-website",
  "version": "1.0.0",
  "description": "An ephemeral project",
  "license": "ISC",
  "author": "Lan",
  "type": "module",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
````

Then we include a similar `index.html` in the tut:

````html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Some website!</tite>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
	</head>
	
	<body>
		<h1>Hi! I am a heading</h1>
	</body>
</html>
````

For some reason `open index.html` is not able to find the existing file at `file:///tmp/del/tut/index.html`... But we can open it in `~/tmp/del/tut`
2025-09-01 Wk 36 Mon - 22:25

Right now the content says `<body></body>` despite us having some content...

But we do see a body with the following content:

````html
<!DOCTYPE html>
<html lang="en">
	<head>
	</head>
	<body>
		<h1>Hi! I am a heading</h1>
	</body>
</html>
````

Even including `<title>Some website!</tite>` inside `<head></head>` would break this...

Oh oops, `</title>` not `</tite>`!

````html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>Some website!</title>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
	</head>
	
	<body>
		<h1>Hi! I am a heading</h1>
	</body>
</html>
````

You can use `view-source:` before a url in firefox to also view it in the browser, or right click and do `View Page Source`.

2025-09-01 Wk 36 Mon - 22:39

Spawn [7.2 SASS language extension inspiration for dbmt](002%20Follow%20with%20wweb%20static%20npm%20website%20tutorial.md#72-sass-language-extension-inspiration-for-dbmt) ^spawn-idea-3f98c7

2025-09-01 Wk 36 Mon - 22:57

````diff
{
  "name": "static-website",
  "version": "1.0.0",
  "description": "An ephemeral project",
  "license": "ISC",
  "author": "Lan",
  "type": "module",
  "main": "index.js",
  "scripts": {
-    "test": "echo \"Error: no test specified\" && exit 1"
+    "css:scss": "node-sass --output-style compressed -o dist src/scss"
  }
}
````

Let's get [Sass](https://sass-lang.com/) for CSS:

````sh
npm i -D node-sass
````

````diff
{
  "name": "static-website",
  "version": "1.0.0",
  "description": "An ephemeral project",
  "license": "ISC",
  "author": "Lan",
  "type": "module",
  "main": "index.js",
  "scripts": {
    "css:scss": "node-sass --output-style compressed -o dist src/scss"
  },
+  "devDependencies": {
+    "node-sass": "^9.0.0"
+  }
}

````

2025-09-01 Wk 36 Mon - 23:30

````sh
# in ~/tmp/del/tut
mkdir -p src/scss
touch src/scss/index.scss
````

````diff
	<head>
+		<link rel="stylesheet", type="text/css", href="dist/index.css">
	</head>
````

2025-09-01 Wk 36 Mon - 23:36

````scss
// in src/scss/_variables.scss
$primary: #16a085;

// in src/scss/index.scss
@import 'variables.scss';

body {
	color: $primary;
}
````

Seems even though it's called `_variables.scss`, we can import it as `variables.scss`, maybe related to it being a compilation flag to ignore the file...

````sh
npm run css:scss
````

2025-09-01 Wk 36 Mon - 23:50

Had to fix a missing semicolon to `@import 'variables.scss'` , but otherwise we're able to build!

It's green!

![Pasted image 20250901235009.png](../../../../../../../../../attachments/Pasted%20image%2020250901235009.png)

2025-09-01 Wk 36 Mon - 23:54

Now we're using [npmjs autoprefixer](https://www.npmjs.com/package/autoprefixer) ([gh postcss/autoprefixer](https://github.com/postcss/autoprefixer)) and [npmjs postcss-cli](https://www.npmjs.com/package/postcss-cli) to manage it.

Install:

````sh
# in ~/tmp/del/tut
npm i -D autoprefixer postcss-cli
````

And also include the new scripts.

````diff
# in ~/tmp/del/tut/package.json
  "scripts": {
-    "css:scss": "node-sass --output-style compressed -o dist src/scss"
+    "css:scss": "node-sass --output-style compressed -o dist src/scss",
+    "css:autoprefixer": "postcss -u autoprefixer -r dist/*.css",
+    "build:css": "npm run css:scss && npm run css:autoprefixer"
  },
  "devDependencies": {
+    "autoprefixer": "^10.4.21",
    "node-sass": "^9.0.0",
+    "postcss-cli": "^11.0.1"
  }
````

2025-09-02 Wk 36 Tue - 00:16

This post [What is Autoprefixer and Why Should You Use It?](https://www.codu.co/articles/what-is-autoprefixer-and-why-should-you-use-it-hwzzvb6i) explains why autoprefixer is needed for cross-browser compatibility where vendor prefixes otherwise would have to be managed manually to ensure a consistent UX across different browsers.

2025-09-02 Wk 36 Tue - 00:25

For linting, get [npmjs stylelint](https://www.npmjs.com/package/stylelint) ([lint rules](https://stylelint.io/user-guide/rules/))

Also getting [npmjs postcss-scss](https://www.npmjs.com/package/postcss-scss)

````sh
npm i -D stylelint postcss-scss
````

2025-09-02 Wk 36 Tue - 05:23

This [post](https://www.sitepoint.com/postcss-sass-configurable-alternative/) explores using postcss-scss for automated configuration.

````diff
# in ~/tmp/del/tut/package.json
  "scripts": {
-	 "build:css": "npm run css:scss && npm run css:autoprefixer"
+    "css:lint": "stylelint src/scss/*.scss  --custom-syntax postcss-scss",
+    "build:css": "npm run css:lint && npm run css:scss && npm run css:autoprefixer",
  },
  "devDependencies": {
+    "postcss-scss": "^4.0.9",
+    "stylelint": "^16.23.1"
  }
````

Create a `.stylelintrc`,

````json
// in ~/tmp/del/tut/.stylelintrc

"rules": {
    "block-no-empty": true,
    "color-hex-case": "lower",
    "color-hex-length": "short",
    "color-no-invalid-hex": true,
    "declaration-colon-space-after": "always",
    "max-empty-lines": 2
}
````

2025-09-02 Wk 36 Tue - 05:33

Adding rebuild automation on file change with [npmjs onchange](https://www.npmjs.com/package/onchange).

````sh
npm i -D onchange
````

````diff
# in ~/tmp/del/tut/package.json
  "scripts": {
-    "build:css": "npm run css:lint && npm run css:scss && npm run css:autoprefixer"
+    "build:css": "npm run css:lint && npm run css:scss && npm run css:autoprefixer",
+    "watch:css": "onchange \"src/scss\" -- npm run build:css"
  },
  "devDependencies": {
+    "onchange": "^7.1.0",
  }
````

Automate away browser refresh with [npmjs browser-sync](https://www.npmjs.com/package/browser-sync),

````sh
npm i -D browser-sync
````

````diff
# in ~/tmp/del/tut/package.json
  "scripts": {
-    "watch:css": "onchange \"src/scss\" -- npm run build:css"
+    "watch:css": "onchange \"src/scss\" -- npm run build:css",
+    "serve": "browser-sync start --server \"dist\" --files \"dist\""
  },

  "devDependencies": {
+    "browser-sync": "^3.0.4",
  }
````

For now, moving `index.html` to `dist` as it is being watched for changes, and make the change

````diff
-		<link rel="stylesheet", type="text/css", href="dist/index.css">
+		<link rel="stylesheet", type="text/css", href="/index.css">
````

2025-09-02 Wk 36 Tue - 05:46

Following some configuration update recommendations,

Use `npm i -D sass` instead of `npm i -D node-sass`

````diff
# in ~/tmp/del/tut/package.json
  "scripts": {
-    "css:scss": "node-sass --output-style compressed -o dist src/scss",
+    "css:scss": "sass --style compressed src/scss:dist",
  },
  "devDependencies": {
-    "node-sass": "^9.0.0",
+    "sass": "^1.91.0",
  }
 
  
# in src/scss/index.scss
-@import 'variables.scss';
+@use 'variables.scss';

body {
-  color: $primary;
+  color: variables.$primary;
}
````

Spawn [7.3 Post Comment integration via utterance.es](002%20Follow%20with%20wweb%20static%20npm%20website%20tutorial.md#73-post-comment-integration-via-utterancees) ^spawn-idea-bb2c9a

2025-09-02 Wk 36 Tue - 06:07

Also we'll use [npmjs concurrently](https://www.npmjs.com/package/concurrently) instead of [npmjs npm-run-all](https://www.npmjs.com/package/npm-run-all)

````sh
npm i -D concurrently
````

And the script

````diff
-"watch": "run-p serve watch:css"
+"watch": "concurrently 'npm run serve' 'npm run watch:css'"
````

````diff
# in ~/tmp/del/tut/package.json
  "scripts": {
-    "serve": "browser-sync start --server \"dist\" --files \"dist\""
+    "serve": "browser-sync start --server \"dist\" --files \"dist\"",
+    "watch": "concurrently 'npm run serve' 'npm run watch:css'"
  },
  "devDependencies": {
+    "concurrently": "^9.2.1",
  }

````

2025-09-02 Wk 36 Tue - 06:27

We run into some lint errors when updating scss:

````
[1] src/scss/_variables.scss
[1]   1:1  ✖  Unknown rule color-hex-case. Did you mean color-hex-alpha, color-hex-length?  color-hex-case
[1]   1:1  ✖  Unknown rule declaration-colon-space-after                                    declaration-colon-space-after
[1]   1:1  ✖  Unknown rule max-empty-lines                                                  max-empty-lines
````

Relevant:

* [stylelint #7400: 4 Rule color-hex-case unexpectedly removed](https://github.com/stylelint/stylelint/issues/7400)

They added migration guides for deprecated rules: [to-16](https://stylelint.io/migration-guide/to-16/#removed-deprecated-stylistic-rules), [to-15](https://stylelint.io/migration-guide/to-15/#deprecated-stylistic-rules)

For deprecation of rules like `color-hex-case`, `declaration-colon-space-after`, `max-empty-lines`  they write:

 > 
 > When we created these rules, pretty printers (like [Prettier](https://prettier.io/)) didn't exist. They now offer a better way to consistently format code, especially whitespace. Linters and pretty printers are complementary tools that work together to help you write consistent and error-free code.

We need to add formatting automation instead. Remove these lints.

````sh
npm install --save-dev --save-exact prettier
````

````diff
# in ~/tmp/del/tut/package.json


  "scripts": {
+    "format": "npx prettier . --check",
-    "build:css": "npm run css:lint && npm run css:scss && npm run css:autoprefixer",
+    "build:css": "npm run format && npm run css:lint && npm run css:scss && npm run css:autoprefixer",
  },
  "devDependencies": {
+    "prettier": "^3.6.2",
  }
````

2025-09-02 Wk 36 Tue - 08:37

Let's use this with pre-commit hooks

Install it if you haven't:

````sh
uv tool install pre-commit
````

Hmm, this [post](https://medium.com/@danielangelesangelestoribio/configure-pre-commit-for-nextjs-project-d511aa6a1f7b) with a recommended pre-commit configuration recommends [biome](https://biomejs.dev/) which integrates formatting and linting

This provides full code linting.  Let's replace prettier with it

````sh
npm i -D --save-exact @biomejs/biome
````

````diff
# in ~/tmp/del/tut/package.json

  "scripts": {
    "css:lint": "stylelint src/scss/*.scss  --custom-syntax postcss-scss",
-    "format": "npx prettier . --check",
+    "biome-check": "npx @biomejs/biome check --write ./src",
    "build:css": "npm run biome-check && npm run css:lint && npm run css:scss && npm run css:autoprefixer",
  },
  "devDependencies": {
+    "@biomejs/biome": "2.2.2",
-    "prettier": "3.6.2",
  }
````

`css:lint` with `stylelint` seems necessary still because it doesn't seem biome handles scss.

2025-09-02 Wk 36 Tue - 09:20

````sh
pre-commit sample-config > .pre-commit-config.yaml
````

Add some extra config

````yaml
# See https://pre-commit.com for more information
# See https://pre-commit.com/hooks.html for more hooks
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
	  - id: check-json
      - id: check-added-large-files
  - repo: local
    hooks:
      - id: local-biome-check
        name: biome check
        entry: npx biome check --write --files-ignore-unknown=true --no-errors-on-unmatched
        language: system
        types: [text]
        files: "\\.(jsx?|tsx?|c(js|ts)|m(js|ts)|d\\.(ts|cts|mts)|jsonc?|svelte|vue|astro|graphql|gql)$"
  - repo: https://github.com/codespell-project/codespell
	rev: v2.4.1
	hooks:
      - id: codespell
````

The biome check config is as recommended in [gh biomejs/pre-commit](https://github.com/biomejs/pre-commit).

And the codespell config is latest as of this writing from [gh codespell-project/codespell](https://github.com/codespell-project/codespell).

2025-09-02 Wk 36 Tue - 09:43

Needed to make sure that `-id: check-json` is not too indented, or `pre-commit run` would give an error. Also needed git repo initialized and node_modules in .gitiginore and files staged to test.

Currently the compression done to css is overridden by biome making the css file pretty. Let's remove `css` from the files to format.

````
codespell................................................................Failed
- hook id: codespell
- exit code: 65

package-lock.json:229: COo ==> coup
package-lock.json:3061: vEw ==> view, vow, vex
package-lock.json:3989: Nd ==> And, 2nd
````

Why is it failing on urls... I guess we need `args: [--skip, package-lock.json]` as recommended by the [post](https://medium.com/@danielangelesangelestoribio/configure-pre-commit-for-nextjs-project-d511aa6a1f7b).

2025-09-02 Wk 36 Tue - 09:58

Back to the [tutorial](https://wweb.dev/blog/how-to-create-static-website-npm-scripts)!

Getting [npmjs imagemin-cli](https://www.npmjs.com/package/imagemin-cli) to minify images

````sh
npm i -D imagemin-cli
````

````diff
# in ~/tmp/del/tut/package.json
	"scripts": {
+		"build:images": "imagemin src/images/**/* --out-dir=dist/images",
+		"watch:images": "onchange \"src/images\" -- npm run build:images",
	},

````

2025-09-02 Wk 36 Tue - 10:04

Spawn [4.1 npm audit reports security vulnerabilities for tutorial template project](002%20Follow%20with%20wweb%20static%20npm%20website%20tutorial.md#41-npm-audit-reports-security-vulnerabilities-for-tutorial-template-project) ^spawn-issue-e23534

2025-09-02 Wk 36 Tue - 10:15

We ran some audit fixes to reduce vulnerablities. This is what we have so far:

````json
	"scripts": {
		"css:scss": "sass --style compressed src/scss:dist",
		"css:autoprefixer": "postcss -u autoprefixer -r dist/*.css",
		"css:lint": "stylelint src/scss/*.scss  --custom-syntax postcss-scss",
		"format": "npx prettier . --check",
		"biome-check": "npx @biomejs/biome check --write ./src",
		"build:css": "npm run biome-check && npm run css:lint && npm run css:scss && npm run css:autoprefixer",
		"build:images": "imagemin src/images/**/* --out-dir=dist/images",
		"build": "npm run build:*",
		"watch:css": "onchange \"src/scss\" -- npm run build:css",
		"watch:images": "onchange \"src/images\" -- npm run build:images",
		"serve": "browser-sync start --server \"dist\" --files \"dist\"",
		"watch": "concurrently 'npm run serve' 'npm run watch:*'"
	},
	"devDependencies": {
		"@biomejs/biome": "2.2.2",
		"autoprefixer": "^10.4.21",
		"browser-sync": "^3.0.4",
		"concurrently": "^9.2.1",
		"imagemin-cli": "^8.0.0",
		"onchange": "^7.1.0",
		"postcss-cli": "^11.0.1",
		"postcss-scss": "^4.0.9",
		"prettier": "3.6.2",
		"sass": "^1.91.0",
		"stylelint": "^16.23.1"
	}
````

2025-09-02 Wk 36 Tue - 11:00

We can use [webpack](https://webpack.js.org/) for bundling and resolving dependencies, and [babeljs](https://babeljs.io/) for writing latest javascript and yet being browser-compatible.

````sh
npm i -D webpack webpack-cli babel-loader @babel/preset-env
````

````
added 224 packages, and audited 865 packages in 33s
````

That's a lot of dependencies.

Let's create the `webpack.config.js` config file in the project root like the [tutorial](https://wweb.dev/blog/how-to-create-static-website-npm-scripts) and add

````json
"build:js": "webpack --mode=production",
"watch:js": "onchange \"src/js\" -- webpack --mode=development",
````

and include the script into the html at the end of the `<body>`:

````html
<script src="./bundle.js"></script>
````

The tutorial uses [eslint](https://www.npmjs.com/package/eslint) but we're already using biome.

Make sure to also have `src/js/main.js`.

2025-09-02 Wk 36 Tue - 11:21

Now let's add the rest of the configuration for building html pages into dist and minifying.

````sh
npm i -D posthtml posthtml-cli posthtml-modules htmlnano
````

Add `posthtml.json` as in the tutorial.

Add build and watch for html:

````json
"build:html": "posthtml -c posthtml.json",
"watch:html": "onchange \"src/views\" -- npm run build:html",
````

Move `dist/index.html` into `src/views/index.html` and split the head into its own file, `src/views/components/head.html` then import it with

````html
<module href="/components/head.html"></module>
````

2025-09-02 Wk 36 Tue - 11:35

````sh
npm run build:html
> static-website@1.0.0 build:html
> posthtml -c posthtml.json

You have to install "cssnano" in order to use htmlnano's "minifyCss" module
You have to install "svgo" in order to use htmlnano's "minifySvg" module
The file /home/lan/tmp/del/tut/src/views/index.html has been saved!
````

2025-09-02 Wk 36 Tue - 11:39

Awesome! We got a lot of tools for modular html, css, js, images, linting, formatting, pre-commit hooks...

One last thing! Let's add to this tutorial by deploying with wasmer!

Let's move this template somewhere more permanent. You can view it in [gh lan-exp-scripts tut](https://github.com/LanHikari22/lan-exp-scripts/tree/main/tutorials/2025/topics/frontend/wwebdev/tut).

Spawn [3.4 Deploy wwebdev tutorial project with wasmer](002%20Follow%20with%20wweb%20static%20npm%20website%20tutorial.md#34-deploy-wwebdev-tutorial-project-with-wasmer) ^spawn-task-7ad6e5

2025-09-02 Wk 36 Tue - 12:19

It's all deployed now!

Let's make add a comment to the tutorial with the process we went through here.

There are a few differences:

* We use [biome](https://biomejs.dev/) instead of eslint for linting and non-scss formatting
* Following [stylelint migration-guide to-15](https://stylelint.io/migration-guide/to-15/#deprecated-stylistic-rules), we removed deprecated rules `color-hex-case`, `declaration-colon-space-after`, `max-empty-lines` since they recommend to just run auto-formatting like with prettifier. We use biome.
* Added pre-commit hooks for formatting and linting
* Added configuration for wasmer deployment

The comment ended up being routed to [Vincenius/wwebdev-comments #3](https://github.com/Vincenius/wwebdev-comments/issues/3).
