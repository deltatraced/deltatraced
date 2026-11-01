---
context_type: investigation
status: done
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned in: [^spawn-invst-797814](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md#spawn-invst-797814)

Resources: [000 Silverbullet Source Resources](../entry/000%20Silverbullet%20Source%20Resources.md)

Overview: [001 Overview SB Option to only have base name in title](../entry/001%20Overview%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

---

# Solution

Silverbullet, `5911ee1e`

In `top_bar.tsx`,

````tsx
<MiniEditor
// text={pageName ?? ""}
text="beep"
````

Gets us to always see `beep` in the title bar.

Then in `editor_ui.tsx`,

````tsx
<TopBar
  pageName={
	!viewState.current ? "" : getNameFromPath(viewState.current.path)
  }
````

We are able to modify the content of `pageName` that would have rendered.

The width of the title bar (as well as the entire editor page) can be set in a page like `website/Space Style.md` via the `--editor-width` setting.

Setting `--editor-width` in theme.scss does correspond to the entire page getting wider, not just the title.

---

# Journal

## How can we hardwire the title text to beep?

2026-06-10 Wk 24 Wed - 08:45 +03:00

`client > content_manager.ts > ContentManager > loadPage`

* `editor_ui.tsx`
  * `import { TopBar } from "./components/top_bar.tsx";`

2026-06-10 Wk 24 Wed - 14:03 +03:00

In `top_bar.tsx`, `5911ee1e`

````tsx
<MiniEditor
// text={pageName ?? ""}
text="beep"
````

Gets us to always see `beep` in the title bar.

## How is pageName loaded to that text prop?

2026-06-10 Wk 24 Wed - 14:11 +03:00

In `editor_ui.tsx`,

````tsx
<TopBar
  pageName={
	!viewState.current ? "" : getNameFromPath(viewState.current.path)
  }
````

2026-06-10 Wk 24 Wed - 14:50 +03:00

We're able to make it just the base name.

In `ref.ts`, add:

````ts
/**
 * Converts a path to just the basename and the extension
 */
export function pathToBasename(path: Path): Path {
  // TODO REVIEW: Maybe we need to check for the OS here, or find a more proper way to do this that does so.
  // because on windows, we would be expecting "\" instead of "/".
  if (path == "") {
    return "";
  }

  let parts = path.split("/");
  let base = parts[parts.length - 1];
  let base_parts = base.split(".")
  let base_no_ext = base_parts[0];
  let ext = base_parts[base_parts.length - 1];

  return `${base_no_ext}.${ext}`;
}
````

In `editor_ui.tsx`, add:

````tsx
import {
  pathToBasename,
} from "@silverbulletmd/silverbullet/lib/ref";

// ...

<TopBar
  pageName={
	// !viewState.current ? "" : getNameFromPath(viewState.current.path)
	!viewState.current ? "" : getNameFromPath(pathToBasename(viewState.current.path))
  }
````

This allows to just render the base name now.

Check [002 SB How do mentions render the page names and strip the path?](002%20SB%20How%20do%20mentions%20render%20the%20page%20names%20and%20strip%20the%20path%3F.md).

It can give us reference as to the solution used there to generate base names.

## What limits the character width of the rendered page name?

2026-06-10 Wk 24 Wed - 16:41 +03:00

`TopBar` $\to$

````tsx
<div
  id="sb-top"
````

In `top.scss`, under `sb-top > .main > .inner`,

````tsx
// Hack to not have SCSS precompile this value but use proper CSS variables
max-width: var(--#{"editor-width"});
````

Setting this to `100px` has the title text very small fitting only `000-;`, and the index loading icon takes away from it when present:

````scss
// Hack to not have SCSS precompile this value but use proper CSS variables
// max-width: var(--#{"editor-width"});
max-width: 100px;
````

`website/Space Style.md` mentions that `editor-width` can be modified.

Otherwise it is currently defined in `theme.scss`,

````scss
--editor-width: 800px;
````
