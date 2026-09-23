---
context_type: task
status: todo
---

Parent: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/000 Getting started ping-pong-score-ts](../000%20Getting%20started%20ping-pong-score-ts.md)

Spawned by: [lan/2026/microproj/001-ping-pong-score-ts/entry/000 Getting started ping-pong-score-ts/000 Getting started ping-pong-score-ts](../000%20Getting%20started%20ping-pong-score-ts.md)

Spawned in: [^spawn-task-368f3a](../000%20Getting%20started%20ping-pong-score-ts.md#spawn-task-368f3a)

Overview: [000 Overview Getting started ping-pong-score-ts](../entry/000%20Overview%20Getting%20started%20ping-pong-score-ts.md)

# Journal

2026-06-21 Wk 25 Sun - 21:10 +03:00

[askubuntu ans Replace a line of text in a file, with the contents of another file](https://askubuntu.com/a/1442893)

2026-06-21 Wk 25 Sun - 21:24 +03:00

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
git commit

# out
[main 335ae32] created ts to js to embedded html pipeline
 Date: Sun Jun 21 21:23:39 2026 +0300
````

2026-06-21 Wk 25 Sun - 21:34 +03:00

[so ans Are custom elements valid HTML5?](https://stackoverflow.com/a/9845124/6944447) $\to$ https://www.w3.org/TR/custom-elements/

https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements

https://developer.mozilla.org/en-US/docs/Web/API/Element/replaceChildren

2026-06-21 Wk 25 Sun - 22:56 +03:00

I remember before we compiled some basic html libraries/build pipelines to use like for css. Need to check.

It should be this, I remembered I created an issue: https://github.com/Vincenius/wwebdev-comments/issues/3

It links to the project: https://github.com/LanHikari22/lan-exp-scripts/tree/main/tutorials/2025/topics/frontend/wwebdev/tut

The notes need updating in the repository. We need to make it a permalink so this never happens. They are in:

[002 Follow with wweb static npm website tutorial](../../../../../../archived/2026-05-21_2025/proj/002%20obsidian-sourced-website/tasks/2025/001%20Create%20a%20reference%20basic%20website%20and%20host%20it%20with%20wasmer/tasks/002%20Follow%20with%20wweb%20static%20npm%20website%20tutorial.md)

Link updated.

Let's take some things from there like the scss, `.pre-commit-config.yaml`, `.stylelintrc`.

though no links in case of scss, we want it compiled into a final `index.html`.

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
cp ~/src/cloned/gh/LanHikari22/lan-exp-scripts/2025/tutorials/topics/frontend/wwebdev/tut/.pre-commit-config.yaml .
cp ~/src/cloned/gh/LanHikari22/lan-exp-scripts/2025/tutorials/topics/frontend/wwebdev/tut/.stylelintrc .
cp ~/src/cloned/gh/LanHikari22/lan-exp-scripts/2025/tutorials/topics/frontend/wwebdev/tut/posthtml.json .
cp ~/src/cloned/gh/LanHikari22/lan-exp-scripts/2025/tutorials/topics/frontend/wwebdev/tut/package.json .
mkdir settings
cp ~/src/cloned/gh/LanHikari22/lan-exp-scripts/2025/tutorials/topics/frontend/wwebdev/tut/settings/config.toml settings/
````

Need to modify some things for this project. We're doing typescript, we should include esbuild pipeline we already have here.

Having issue with `npm run build` referencing `build:*` but it doesn't seem to pick up that glob pattern.

https://docs.npmjs.com/cli/v10/configuring-npm/package-json?v=true

1. https://docs.npmjs.com/cli/v10/configuring-npm/package-json?v=true#scripts
1. https://docs.npmjs.com/cli/v10/using-npm/scripts

We see no reference of this `*` usage. Also in the referenced [tutorial](https://wweb.dev/blog/how-to-create-static-website-npm-scripts) in the note they are using `run-p` to do these glob patterns:

````sh
npm i -D npm-run-all
````

2026-06-22 Wk 26 Mon - 00:50 +03:00

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
git commit

# out
[main 78a86e5] add some project facing files
 3 files changed, 44 insertions(+)
 create mode 100644 CONTRIBUTING.md
 create mode 100644 FUNDING.yml
 create mode 100644 LICENSE
````

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
git commit

# out
[main ea90ec9] add README
````

2026-06-22 Wk 26 Mon - 09:58 +03:00

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/ping-pong-score-ts
npm i -D cssnano
npm i -D tsc

# out
npm warn deprecated tsc@2.0.4: Package no longer supported. Contact Support at https://www.npmjs.com/support for more info.
````

We need a setup phase. We'll run this in docker to get a deterministic list of things to install.

[000 Configure typechecking and other lints for typescript PPST](000%20Configure%20typechecking%20and%20other%20lints%20for%20typescript%20PPST.md)
