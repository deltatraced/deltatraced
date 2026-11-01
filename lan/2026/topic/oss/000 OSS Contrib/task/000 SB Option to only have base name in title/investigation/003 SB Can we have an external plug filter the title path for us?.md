---
context_type: investigation
status: done
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned in: [^spawn-invst-96388f](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md#spawn-invst-96388f)

Overview: [001 Overview SB Option to only have base name in title](../entry/001%20Overview%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

---

# Problem

One of the proposals for this problem is instead of creating a config under Std, we allow any Plug to modify the title of the page as they see fit. A plug can subscribe to the event and give us the desired result filtered title. This proposal allows a plug for extra options to be used, in case this sort of change is deemed unnecessary in Std. A counterpoint is that this behavior: Of having a title without the path, may also be small enough and preferable enough by many that it ought be a standard configuration option. However, I do not determine whether this is the case, so we pursue here how to implement this proposal.

I describe the proposal in https://github.com/silverbulletmd/silverbullet/issues/2016.

# Solution

In accordance with the changes introduced in [004 SB How can we pass async determined data for the page title?](004%20SB%20How%20can%20we%20pass%20async%20determined%20data%20for%20the%20page%20title%3F.md), we are able to use the main UI state as well as a new defined service in the Std plug to customize the title page.

Because it is a service, it should be possible to handle conflicting accounts via priority.

---

# Journal

2026-06-11 Wk 24 Thu - 01:03 +03:00

Spawn [002 Issues during Can we have an external plug filter the title path for us?](../entry/002%20Issues%20during%20Can%20we%20have%20an%20external%20plug%20filter%20the%20title%20path%20for%20us%3F.md) ^spawn-entry-dd47e7

## Can I create a new event and subscribe to it from STD?

2026-06-10 Wk 24 Wed - 18:55 +03:00

Example code that uses an event to accept input:

https://github.com/silverbulletmd/silverbullet/blob/342686b2fc79df2487e1ab36b49216958edcaee4/client/content_manager.ts#L317

````ts
      // Let's dispatch a editor:pageCreating event to see if anybody wants to do something before the page is created
      const results = (await this.client.dispatchAppEvent(
        "editor:pageCreating",
````

Used in `libraries/Library/Std/APIs/Virtual Page.md`, which specifies how to autogenerate pages that don't actually exist on disk.

2026-06-10 Wk 24 Wed - 19:07 +03:00

`website/Service.md` this concept is close to what we want.

If we create a new event like `editor:pageCreating`, we need to add it to `website/Event.md` as documentation, and to the `AppEvent` type in `plug-api/types/client.ts`, and also a record type for the parameters in `plug-api/types/event.ts`.

Example:

In `plug-api/types/event.ts`,

````ts
import { Path } from "../lib/ref.ts";

export type PageTitleRender = {
  path: Path;
}
````

Though it seems more fit to use a service here, so that we can provide a default implementation and allow plugs to override it. See examples in `client/service_registry.test.ts`.

In `client/space_lua/stdlib/net.ts`,

The core code has an example of  a service being used:

````ts
  readURI: new LuaNativeJSFunction(
    (uri: string, options: { uri?: string; encoding?: string } = {}) => {
      options.uri = uri;
      return client.clientSystem.serviceRegistry.invokeBestMatch(
        `net.readURI:${uri}`,
        options,
      );
    },
  ),
````

In `libraries/Library/Std/Infrastructure/Github.md`,

we can see that multiple services are defined like

````
  selector = "net.readURI:github:*",
  selector = "net.readURI:ghr:*",
````

So they can all be invoked independently from the same endpoint with different url. In our case, we need to provide a unique service, and differentiate by priority in match:

````
  match = { priority=1 },
````

with 1 being lowest priority, so other plugs can override.

For example, we can add under a new page `libraries/Library/Std/Pages/Title Render.md`, using `space-lua` instead of `lua`:

````lua
service.define {
  selector = "renderPageTitle",
  match = {
    priority=1 -- Other plugs may override this, although only two subscribers (one besides Std) is allowed
  },
  -- (path: Path): String
  run = function(path) 
    return "Beeep"
  end
}
````

Though the issue now is we are not preventing duplicates, only sorting through them. But still, given that Std has an implementation, we can have it so at most two subscribers exist.

2026-06-11 Wk 24 Thu - 00:36 +03:00

* As mentioned in [000 SB How is the title rendered when a page is loaded?](000%20SB%20How%20is%20the%20title%20rendered%20when%20a%20page%20is%20loaded%3F.md),
  * `client/editor_ui.tsx > TopBar > pageName`

Instead of

````tsx
!viewState.current ? "" : getNameFromPath(pathToBasename(viewState.current.path))
````

(This prompted an investigation as I realized we can't pass async data directly. See [002 Issues during Can we have an external plug filter the title path for us?](../entry/002%20Issues%20during%20Can%20we%20have%20an%20external%20plug%20filter%20the%20title%20path%20for%20us%3F.md))

we update with async retrieved state as in [004 SB How can we pass async determined data for the page title?](004%20SB%20How%20can%20we%20pass%20async%20determined%20data%20for%20the%20page%20title%3F.md).

Because now we're able to get data directly from the defined service, we are in fact able to update the page in accordance with a plugin event!
