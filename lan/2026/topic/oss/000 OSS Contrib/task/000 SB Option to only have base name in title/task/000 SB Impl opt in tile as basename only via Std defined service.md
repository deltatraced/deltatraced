---
context_type: task
status: todo
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned in: [^spawn-task-287814](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md#spawn-task-287814)

Overview: [001 Overview SB Option to only have base name in title](../entry/001%20Overview%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

# Journal

2026-06-12 Wk 24 Fri - 17:57 +03:00

vscode seems to have many undesirable formatting changes in the pr that we should not have. Let's reset and add the changes again.

Latest changes in accordance with the diffs in [004 SB How can we pass async determined data for the page title?](../investigation/004%20SB%20How%20can%20we%20pass%20async%20determined%20data%20for%20the%20page%20title%3F.md)

Reset the relevant pages:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git checkout HEAD -- ./plug-api/lib/ref.ts
git checkout HEAD -- ./client/editor_ui.tsx
git checkout HEAD -- ./client/types/ui.ts
````

Let's also modify the configuration name,

````lua
# in ./libraries/Library/Std/Config.md
config.define("StripPagePathInTitle", {
  description = "Page titles by default show the entire path to the page. This shows the name only.",
  type = "boolean",
  default = false,
  ui = { category = "Editor", label = "Strip page path in title", priority = 1 },
})
````

And add back the changes, although ensuring no other unnecessary formatting changes are introduced.

Spawn [000 SB Services getting initialized multiple times and getting uninitialized between renders for page title](../issue/000%20SB%20Services%20getting%20initialized%20multiple%20times%20and%20getting%20uninitialized%20between%20renders%20for%20page%20title.md) ^spawn-issue-b202e7

2026-06-13 Wk 24 Sat - 22:24 +03:00

We should be able to just write the lua script now in Std to implement the customized title. We confirm we're able to get the config value and give one of two values based on whether the option is enabled.

How is it that when I kill the air process running silverbullet, my browser still has something at localhost:3000?

`sudo ss -tulpn | less` does not show it, and neither does `sudo lsof -i :3000`.

Note though if we run `air ~/src/cloned/cb/deltatraced/deltatraced/`, then we do get a reading with `sudo lsof -i :3000` and `sudo ss -tulpn | grep ":3000"`.

So it seems to just be cached by the browser. We're able to find that it no longer is accessible in incognito for firefox after killing air. It still caches incognito, unless you close it.

We should also make it so we default to just displaying the full path name if for whatever reason, the service is currently unavailable. We shouldn't leave the user with an empty title bar.

2026-06-14 Wk 24 Sun - 13:28 +03:00

Next issue is that the title is not updating on page load now when we navigate. `client/client.ts > initNavigator`.

It's probably this:

````ts
// in client/editor_ui.tsx > fn ViewComponent()
    useEffect(() => {
      // Ask a service to possibly customize the title, or otherwise just use the path.
      customizePageTitleViaService()
        .then(title => {
            viewState.pageTitle = title;
        })
    }, [viewState.current === undefined, ]);
````

The dependency `Viewstate.current === undefined` will not be able to tell when the current path updates.

````diff
// in client/editor_ui.tsx > fn ViewComponent()
         <TopBar
-          pageName={
-            !viewState.current ? "" : getNameFromPath(viewState.current.path)
-          }
+          pageName={viewState.pageTitle}
````

Before, because it is used directly in rendering, it is sensitive to `viewState.current.path`.

````ts
// in client/editor_ui.tsx > fn ViewComponent()
    useEffect(() => {
      console.log(`AAA00 useEffect customizePageTitleViaService (cur (j ${JSON.stringify(viewState.current)}))`);
      // Ask a service to possibly customize the title, or otherwise just use the path.
      customizePageTitleViaService()
````

So this updates when we move pages, but we're not seeing the title update.

````ts
// in plug-api/lib/ref.ts > fn customizePageTitleViaService
const services = await client.clientSystem.serviceRegistry.discover("customizePageTitle", path);
console.log(`AAA01 customizePageTitleViaService (services (j ${JSON.stringify(services)})) (path ${path})`);
````

These trigger consistently.

Updating log

````ts
// in client/editor_ui.tsx > fn ViewComponent()
    useEffect(() => {
      console.log(`AAA00a useEffect customizePageTitleViaService (path (j ${JSON.stringify(!viewState.current ? "" : viewState.current.path)}))`);
      // Ask a service to possibly customize the title, or otherwise just use the path.
      customizePageTitleViaService()
        .then(title => {
            console.log(`AAA00b useEffect customizePageTitleViaService (title ${title})`);
            viewState.pageTitle = title;
        })
    }, [!viewState.current ? "" : viewState.current.path, ]);
````

The logs trigger consistently but the page rendering is inconsistent in updating. `AAA00b` gets us the correct title, which should be set to `viewState.pageTitle` but nonetheless it will sometimes fail to update on the page.

Let's force an update with `client.editorView.dispatch({});`

Spawn [005 SB How does viewDispatch work?](../investigation/005%20SB%20How%20does%20viewDispatch%20work%3F.md) ^spawn-invst-b898d2

2026-06-14 Wk 24 Sun - 19:47 +03:00

Yeah that use of `client.editorView.dispatch` is wrong. We did not have an explicit mechanism for updating this new state, and `client.editorView.dispatch`  is a transaction to codemirror! We wanted `this.viewDispatch`:

````ts
// in client/editor_ui.tsx > fn ViewComponent()
    useEffect(() => {
      console.log(`AAA00a useEffect customizePageTitleViaService (path (j ${JSON.stringify(!viewState.current ? "" : viewState.current.path)}))`);
      // Ask a service to possibly customize the title, or otherwise just use the path.
      customizePageTitleViaService()
        .then(title => {
            console.log(`AAA00b useEffect customizePageTitleViaService (title ${title})`);
            this.viewDispatch({ type: "set-page-title", pageTitle: title });
        })
    }, [!viewState.current ? "" : viewState.current.path, ]);
````

It's much more responsive now!

2026-06-14 Wk 24 Sun - 20:07 +03:00

This seems to be the feature! There are some other things to look into, like when we use `[[]]` it shows a long path and it's not really usable. But that case is a bit more complicated as there can possibly be conflicts between the names. Although Silver Bullet warns on these cases and has a mechanism to detect conflict, so it is something that is likely doable.

For now, let us test the application in accordance with https://github.com/silverbulletmd/silverbullet/blob/main/CONTRIBUTING.md:

````
make test
make fmt
````

For some reason `make fmt` did a lot of changes to the repo... and I made sure to edit it to have only my changes show up. Either way, protocol.

No, some of those changes are really bad. I will have to commit them on their own as `make fmt` changes, and they need to update the protocol if they find it undesirable. Unfortunately I'm going to have to look for my own changes now. Here goes:

````lua
// in libraries/Library/Std/Pages/Name Customizations.md
#meta

This page implements functionality for overriding default title rendered on the top bar of a page

-- change $$$ back to the backticks in code
$$$space-lua
service.define {
  selector = "customizePageTitle",
  match = {
     -- Other plugs of higher priority may override this behavior. 
     -- Std allows via configuration full path name of base path name.
    priority=1
  },

  -- path here is without the extension.
  -- (path: string): string
  run = function(path) 
    local opt = config.get("StripPagePathInTitle", false);
    if not opt then
        return path
    end

    if path == "" then
        return ""
    end

    local parts = string.split(path, "/");
    local base = parts[# parts]

    return base;
  end
}
$$$
````

````ts
// in libraries/Library/Std/Config.md
config.define("shortWikiLinks", {
	// ...
})

config.define("StripPagePathInTitle", {
  description = "Page titles by default show the entire path to the page. This shows the name only.",
  type = "boolean",
  default = false,
  ui = { category = "Editor", label = "Strip page path in title", priority = 1 },
})
````

````ts
// in client/types/ui.ts > AppViewState
  // Possibly customized page title via a plug
  pageTitle?: string;

// in client/types/ui.ts > initialViewState
  pageTitle: "",
  
// in client/types/ui.ts > Action
  | { type: "set-page-title"; pageTitle: string };
````

````ts
// in client/reducer.ts > fn reducer
case "set-page-title":
  return {
	...state,
	pageTitle: action.pageTitle,
  };
````

````ts
// in client/editor_ui.tsx
import {
  customizePageTitleViaService,
} from "@silverbulletmd/silverbullet/lib/ref";

// in client/editor_ui.tsx > fn ViewComponent
    useEffect(() => {
      // Ask a service to possibly customize the title, or otherwise just use the path.
      customizePageTitleViaService().then((title) => {
        this.viewDispatch({ type: "set-page-title", pageTitle: title });
      });
    }, [!viewState.current ? "" : viewState.current.path]);
	
// in client/editor_ui.tsx > fn ViewComponent
        <TopBar
          pageName={viewState.pageTitle}
````

````ts

// in plug-api/lib/ref.ts
/**
 * A service with selector `customizePageTitle` has a default impl in the Std plug (priority 1), which may be
 * overridden by other plugs. The service is provided a ref encoded ({@link encodeRef}) path of the current page.
 */
export async function customizePageTitleViaService(): Promise<string> {
  if (client.ui.viewState.current === undefined) {
    return new Promise(function (resolve, _reject) {
      resolve("");
    });
  } else {
    let path = getNameFromPath(client.ui.viewState.current.path);

    const services = await client.clientSystem.serviceRegistry.discover(
      "customizePageTitle",
      path,
    );

    if (services.length === 0) {
      return new Promise(function (resolve, _reject) {
        // Just give the path until we have service. This can happen during big index jobs
        resolve(path);
      });
    } else {
      return await client.clientSystem.serviceRegistry.invoke(
        services[0],
        path,
      );
    }
  }
}
````

2026-06-14 Wk 24 Sun - 20:29 +03:00

Alright. Let's `git reset --hard` and reimplement this, test it myself, then run the tests, and commit. Then format, and commit. Then open a PR!

Okay. All good with manual and automatic testing.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git commit

# out
[title-text-config a86350da] Add option to strip title full path to name via service (#2016)
 6 files changed, 92 insertions(+), 4 deletions(-)
 create mode 100644 libraries/Library/Std/Pages/Name Customizations.md
````

and for the tests with `make test`,

````
 Test Files  76 passed (76)
      Tests  910 passed (910)
   Start at  20:45:30
   Duration  3.23s (transform 18.46s, setup 0ms, import 43.59s, tests 4.93s, environment 20ms)
````

Now let's run the formatter as well.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/silverbulletmd/branches/silverbullet@title-text-config
git commit -m "chore: run make fmt"

# out
[title-text-config 528b5741] chore: run make fmt
 118 files changed, 2888 insertions(+), 2620 deletions(-)
````

This is quite involved. Will have to mention if we need to reverse this.

Now we need to make sure these notes themselves are up to date to be included in the PR, right from the [001 Overview SB Option to only have base name in title](../entry/001%20Overview%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md).
