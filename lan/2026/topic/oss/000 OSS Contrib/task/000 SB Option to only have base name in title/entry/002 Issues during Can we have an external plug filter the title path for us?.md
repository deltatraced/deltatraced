---
context_type: entry
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [003 SB Can we have an external plug filter the title path for us?](../investigation/003%20SB%20Can%20we%20have%20an%20external%20plug%20filter%20the%20title%20path%20for%20us%3F.md)

Spawned in: [^spawn-entry-dd47e7](../investigation/003%20SB%20Can%20we%20have%20an%20external%20plug%20filter%20the%20title%20path%20for%20us%3F.md#spawn-entry-dd47e7)

# Journal

## We can't put async plug code in page rendering directly

2026-06-11 Wk 24 Thu - 01:05 +03:00

    ## TODO Can I create a new event and subscribe to it from STD?
    
    2026-06-11 Wk 24 Thu - 00:36 +03:00
    
    - As mentioned in [[000 Silverbullet How is the title rendered when a page is loaded?]],
    	- `client/editor_ui.tsx > TopBar > pageName`
    
    Instead of 
    
    ```tsx
    !viewState.current ? "" : getNameFromPath(pathToBasename(viewState.current.path))
    ```
    
    We use
    
    ```ts
    import {
      renderPageTitle,
    } from "@silverbulletmd/silverbullet/lib/ref";
    
    // ...
    
    !viewState.current ? "" : renderPageTitle(viewState.current.path)
    ```
    
    with `renderPageTitle` defined in `plug-api/lib/ref.ts` as
    

We run into an issue here. `renderPageTitle` will need to be async, since services are async. However, `ViewComponent()` is synchronous!

It may be better for plug code to have the option to be synchronous, because in this case, we are not doing anything resource intensive.

Spawn [004 SB How can we pass async determined data for the page title?](../investigation/004%20SB%20How%20can%20we%20pass%20async%20determined%20data%20for%20the%20page%20title%3F.md) ^spawn-invst-bd4095
