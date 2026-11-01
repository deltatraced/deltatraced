# 1 Journal

* [x] Reject

From [^spawn-issue-d4b55b](001%20Errors%20while%20attempting%20to%20import%20zeromq%20for%20non-node%20platform.md#spawn-issue-d4b55b) in [3.4 Send web messages to terminal to collect experiment data](001%20Errors%20while%20attempting%20to%20import%20zeromq%20for%20non-node%20platform.md#34-send-web-messages-to-terminal-to-collect-experiment-data)

2025-07-30 Wk 31 Wed - 13:00

Ran into this issue while trying to build the pub/sub example for [zeromq.js](https://zeromq.github.io/zeromq.js/).

````sh
npm install zeromq
````

````ts
✘ [ERROR] Could not resolve "fs"

    node_modules/cmake-ts/build/loader.js:1:116:
      1 │ ...tringTag,{value:"Module"});const g=require("module"),u=require("fs"),h=require("path");var l=typeof document<"u"?document.currentS...
        ╵                                                                   ~~~~

  The package "fs" wasn't found on the file system but is built into node. Are you trying to bundle
  for node? You can use "--platform=node" to do that, which will remove this error.
````

These are the available platforms

````sh
npx esbuild --help | less

# out (relevant) 
  --platform=...        Platform target (browser | node | neutral,
                        default browser)
````

Our script runs directly on the browser for this game.

This seems node only, so let's look at another option.

### 1.1.1 Reject
