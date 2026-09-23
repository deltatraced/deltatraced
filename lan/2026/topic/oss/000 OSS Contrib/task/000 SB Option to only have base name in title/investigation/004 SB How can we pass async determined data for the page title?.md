---
context_type: investigation
status: done
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [002 Issues during Can we have an external plug filter the title path for us?](../entry/002%20Issues%20during%20Can%20we%20have%20an%20external%20plug%20filter%20the%20title%20path%20for%20us%3F.md)

Spawned in: [^spawn-invst-bd4095](../entry/002%20Issues%20during%20Can%20we%20have%20an%20external%20plug%20filter%20the%20title%20path%20for%20us%3F.md#spawn-invst-bd4095)

# Problem

We need to get the page title asynchronously, and then update the TopBar page prop accordingly.

# Solution

There already is shared Main UI state. We can have it store the page to be rendered, then use an effect to eventually capture the new value. Otherwise leave as empty.

# Journal

2026-06-11 Wk 24 Thu - 01:05 +03:00
One way to get around this is to use `useEffect` inside the page rendering code, but we should look for precedent on if it's ever used with async.

* `client/boot.ts`
  
  * imports `client/client.ts`
    * On boot, will run `async client.init`.
    * `client/client.ts`
      * imports `client/editor_ui.ts > MainUI`
        * Runs `this.ui.render(this.parent)` on it in `async fn Client::init`
        * `client/editor_ui.ts > MainUI`
          * imports `client/components/top_bar.tsx > TopBar`
            * Rendered to HTML in `fn ViewComponent`
            * `client/components/top_bar.tsx > TopBar`
      * imports `client/client_system.ts`
        * Runs `this.clientSystem.init();` in `async fn Client::init`
        * `client/client_system.ts`
      * imports `client/content_manager.ts`
        * Subscribes to `on page load` through `this.pageNavigator.subscribe` via `Client::initNavigator` and runs `await this.contentManager.loadPage` in it.
        * `client/content_manager.ts`
* In javascript it seems enough to write `constructor(private my_field) { }`. Then once initialized, you have access to `this.my_field`. I'm used to decoupling the parameters of a constructor from the fields of a struct.

Per https://preactjs.com/tutorial/01-vdom, the syntax like `<TopBar ...` in `client/editor_ui.tsx` is using something like JSX to be compiled rather than direct javascript/typescript, which is converted to `VDOM` created via `createElement` from preact. `render` then is used to create the actual `DOM`.

Reading through the tutorials for preactjs to get an idea of the general use cases

2026-06-12 Wk 24 Fri - 12:25 +03:00

````ts
//	useEffect(async () => {
//    let newTodos = await getTodos();
//		setTodos(newTodos);
//    alert(`3 newTodos: ${JSON.stringify(newTodos)}`);
//	}, []);

	useEffect(() => {
	    getTodos().then(newTodos => {
	      setTodos(newTodos);
	      alert(`3 newTodos: ${JSON.stringify(newTodos)}`);
	    });
	}, []);
````

This is one example I wrote for the tutorial that we can do something like. There's some state for the component defined as `	const [todos, setTodos] = useState([]);`, and we use an effect and we don't care to wait, whenever it is eventually set via `Promise<T>::then` a state update on the component will be followed by a render update.

So we add to the `AppViewState` some relevant state,

````ts
// Possibly transformed page title via a plug
pageTitle?: string,
````

with the following initial value,

````ts
export const initialViewState: AppViewState = {
// ...
  pageTitle: "",
}
````

Now our state is available through `viewState` in `ViewComponent`. We add an extra `useEffect` in `ViewComponent` to ask for the service filtering the title. This doesn't need to await because re-rendering will trigger whenever it happens. We don't need dependencies in the `useEffect` second list arg, it should only happen once when this is initially rendered.

2026-06-12 Wk 24 Fri - 17:22 +03:00

We're able to get the service to modify the title bar asynchronously now. This is what we have so far:

Commit `5911ee1e`,

````lua
-- in libraries/Library/Std/Pages/Name Customizations.md
-- replace $$$ with backticks.
#meta

This page implements functionality for overriding default title rendered on the top bar of a page

$$$space-lua
service.define {
  selector = "customizePageTitle",
  match = {
    priority=1 -- Other plugs may override this, although only two subscribers (one besides Std) is allowed
  },

  -- path here is without the extension.
  -- (path: string): string
  run = function(path) 
    return "Beeep"
  end
}
$$$
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
      resolve("")
    });
  } else {
    let path = getNameFromPath(client.ui.viewState.current.path);

    return await client.clientSystem.serviceRegistry.invokeBestMatch("customizePageTitle", path);
  }
}
````

````ts
// in client/editor_ui.tsx

	// in ViewComponent()
    useEffect(() => {
      // Ask a service to possibly customize the title, or otherwise just use the path.
      customizePageTitleViaService()
        .then(title => {
            viewState.pageTitle = title;
        })
    }, [viewState.current]);

	// ...
	
	// in ViewComponent()
		<TopBar
	  pageName={
		// !viewState.current ? "" : getNameFromPath(viewState.current.path)
		viewState.pageTitle
````

````ts
// in client/types/ui.ts
export type AppViewState = {
  // ...
  // Possibly customized page title via a plug
  pageTitle?: string, 
}
export const initialViewState: AppViewState = {
  // ...
  pageTitle: "",
}
````

````ts
// in libraries/Library/Std/Config.md
config.define("onlyShowPageNameForPageTitle", {
  description = "Page titles by default show the entire path to the page. This shows the name only.",
  type = "boolean",
  default = false,
  ui = { category = "Editor", label = "Only show name for page title", priority = 1 },
})
````
