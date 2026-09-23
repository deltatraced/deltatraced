---
context_type: task
status: todo
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-task-1f7f9a](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-task-1f7f9a)

# Journal

2026-07-28 Wk 31 Tue - 08:51 +03:00

I used to use vim and vscode. Let's try something else.

https://www.reddit.com/r/HelixEditor/comments/12ah0w7/what_do_you_love_about_helix_especially_compared/

https://www.reddit.com/r/vim/comments/opvv66/should_i_use_vim_or_neovim/

Spawn [000 How can we enable shell scripts to source recursively in a given directory?](../investigation/000%20How%20can%20we%20enable%20shell%20scripts%20to%20source%20recursively%20in%20a%20given%20directory%3F.md) ^spawn-invst-cc6f66

2026-08-02 Wk 31 Sun - 11:54 +03:00

https://wiki.gentoo.org/wiki/Kakoune

https://gigasblade.blogspot.com/2025/10/helix-vs-kakoune-october-2025-which.html

https://github.com/helix-editor/helix/discussions/2138 `Helix and Kakoune`

https://github.com/helix-editor/helix/discussions/3806 `Plugin system`

Still no (official) plugin system with helix.

From https://helix-editor.com/ ,

````
### What about plugins?

While there is currently no plugin system available, we do intend to eventually have one. But this will take some time ([more discussion here](https://github.com/helix-editor/helix/discussions/3806)).

### How does it differ from Kakoune?

Mainly by having more things built-in. Kakoune is [composable by design](https://github.com/mawww/kakoune/blob/master/doc/design.asciidoc), relying on external tooling to manage splits and provide language server support. Helix instead chooses to integrate more. We also use tree-sitter for highlighting and code analysis.

### How does it differ from Vim?

By starting from scratch we were able to learn from our experience with Vim and make some breaking changes. The result is a much smaller codebase and a modern set of defaults. It's easier to get started if you've never used a modal editor before, and there's much less fiddling with config files.
````

* (1) https://github.com/helix-editor/helix/discussions/13945 `Plugin system prototype for Helix`
* (2) https://github.com/helix-editor/helix/pull/8675 `Add Steel as an optional plugin system`
* https://sraj.me/notes/helix-steel/ `Setting up a basic steel plugin for Helix`
* https://github.com/mattwparas/helix/blob/steel-event-system/STEEL.md
* https://github.com/npupko/awesome-helix
* https://www.reddit.com/r/HelixEditor/comments/1bfxgdv/is_there_a_timeline_for_plugin_system_and_release/

So it seems that a steel (lisp-like) plugin (2) is most likely, though someone tried to offer an `alternative (1)` that doesn't rely on a script system runtime.

2026-08-02 Wk 31 Sun - 12:40 +03:00

What about kakoune's plugin system? What are the available options?

* https://kakoune.org/plugins.html
* https://codeberg.org/jdugan6240/kak-bundle
* (3) https://github.com/kakoune-lsp/kakoune-lsp

`kakoune-lsp (3)` is largely build in rust.

2026-08-02 Wk 31 Sun - 13:24 +03:00

* https://strongly-typed-thoughts.net/blog/vim-kakoune-puzzles-2025 `Vim vs. Kakoune puzzles`
  * Has different use cases comparing how an action is done in kakoune vs vim

Spawn [lan/2026/main/task/001 Install a new Gentoo system/entry/004 Learning to use kak](../entry/004%20Learning%20to%20use%20kak.md) ^spawn-entry-717e09

2026-08-02 Wk 31 Sun - 15:13 +03:00

* https://www.akamai.com/cloud/guides/writing-a-vim-plugin

2026-08-02 Wk 31 Sun - 15:15 +03:00

* https://github.com/dradtke/neovim-rs
  * This only does so over TCP
* https://www.reddit.com/r/rust/comments/xxa562/a_neovim_previewer_plugin_written_in_rust/
* $\to$ https://github.com/KillTheMule/nvim-rs
  * Uses https://neovim.io/doc/user/api/, so also over TCP
