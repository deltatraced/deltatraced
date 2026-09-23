---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/entry/006 Configuring nvim on new gentoo install](006%20Configuring%20nvim%20on%20new%20gentoo%20install.md)

Spawned in: [^spawn-entry-0ac83f](006%20Configuring%20nvim%20on%20new%20gentoo%20install.md#spawn-entry-0ac83f)

# Journal

2026-08-03 Wk 32 Mon - 14:54 +03:00

Setting up Rust LSP,

(1) https://www.rustfaq.org/en/how-to-set-up-neovim-for-rust-development/

Setup will go to `/home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init.lua`, which is sourced in `~/.config/nvim/init.vim` via `/home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/install.sh`.

(aside)

Oh! win+b can make a pane nestable so that it can be split. And this can repeat recursively. At least sometimes? Didn't work with just a workspace with foot panes. But also the changing between split types didn't work either. Actually it works now. Not sure what that was. It sometimes happens on some windows but then moving around and repeating resolves it.

(/aside)

2026-08-03 Wk 32 Mon - 19:47 +03:00

We get `module 'lspconfig' not found`.

https://github.com/neovim/nvim-lspconfig

They deprecated `require('lspconfig')` as of `nvim 0.11+`. (Mine: `nvim --version # out (relevant) { NVIM v0.11.7 }`.)

They mention that as of `nvim 0.12+` there is a built-in `vim.pack` plugin manager. But I don't have access to this. Let's upgrade: [002 Quick new Installs for Gentoo System](002%20Quick%20new%20Installs%20for%20Gentoo%20System.md)

`nvim --version # out (relevant) { NVIM v0.12.3 }`

Adding

````lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/plugin/init.lua
vim.pack.add{ { src = 'https://github.com/neovim/nvim-lspconfig' }, }
````

2026-08-03 Wk 32 Mon - 21:33 +03:00

https://github.com/rust-lang/rust-analyzer

Adding config from https://rust-analyzer.github.io/book/other_editors.html#nvim-lsp

Install rust-analyzer: [002 Quick new Installs for Gentoo System](002%20Quick%20new%20Installs%20for%20Gentoo%20System.md)

2026-08-03 Wk 32 Mon - 23:08 +03:00

Now with `:lsp enable` and `:lsp restart` in a rust source code file, we get the type overlays!

We can `ctrl+]` into symbols in our project, but not currently for other crates. We can see documentation for a symbol with `K`. Also if you do `KK` it lets you scroll the popup which is super cool! Always had trouble with that in vscode forcing me to go use the mouse, or just the popup disappearing on very sensitive mouse movements. You can quit the focused popup with `:q` (or just `q`).

2026-08-04 Wk 32 Tue - 00:18 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles
git commit

# out
[main 03f1450] configure rust lsp for nvim
````

2026-08-04 Wk 32 Tue - 00:26 +03:00

https://rust-analyzer.github.io/book/configuration.html

Actually. I'm not able to `cargo build` for the project I am testing on, so this might be why we're not getting other crate symbols.

Yup. Works fine after a successful build.

https://stackoverflow.com/a/73842679 - query active key bindings with `:map`.

We can show diagnostics with `C-w d`.  Same as `KK`, you can do `C-w d C-w d` to make it stick.

For smart autocomplete, we can type some of what we want and then do `C-x C-o`. It nicely shows us the signature too.

We can perform code actions, like importing, with `gra`.

2026-08-04 Wk 32 Tue - 11:53 +03:00

* [x] https://neovim.io/doc/user/lsp/#lsp-autocompletion
* $\to$  https://neovim.io/doc/user/lsp/#lsp-attach

Not sure if I want autocompletion, it seems to do the same thing as `C-x C-o` but is auto triggered? Anyway we can use `C-y` to make a selection from that menu.

I would like to disable autotyping the first selection: https://stackoverflow.com/questions/74688630/make-nvim-cmp-not-autoselect-the-1st-option

The reason is that you can incrementally narrow down options with `C-x C-o`, and it's rare I want the first option.

https://lsp-devtools.readthedocs.io/en/latest/capabilities/text-document/completion.html

2026-08-04 Wk 32 Tue - 13:02 +03:00

Where is `vim.lsp.completion`?

* Search commits for `vim.lsp`

* $\to$ https://github.com/neovim/neovim/commit/929e644a5a94497f1234a2b3b28abe5bbc8066ee

* $\to$ https://github.com/neovim/neovim/blob/master/runtime/lua/vim/lsp/completion.lua

* https://github.com/neovim/neovim/blob/a29297ac538f9a86bbcd5e59cc8a9e3494b263c6/runtime/lua/vim/lsp/completion.lua#L1319 `vim.lsp.completion.enable`

We can see the `autotrigger `option we explicitly set disabled

````lua
# in https://github.com/neovim/neovim/blob/a29297ac538f9a86bbcd5e59cc8a9e3494b263c6/runtime/lua/vim/lsp/completion.lua#L1220
--- @field autotrigger? boolean  (default: false) When true, completion triggers automatically based on the server's `triggerCharacters`.
````

Via https://github.com/neovim/neovim/blob/a29297ac538f9a86bbcd5e59cc8a9e3494b263c6/runtime/lua/vim/lsp/completion.lua#L11,

````
:set completeopt+=menuone,noselect,popup
````

It also mentioned this in the docs here: https://neovim.io/doc/user/lsp/#lsp-completion

Yay! This brings back the full menu that narrows down as you type!

2026-08-04 Wk 32 Tue - 15:41 +03:00

`grn` to rename a symbol

`]d` and `[d` to cycle through diagnostic marks.

`grr` to select references to go to for symbol, and close the quickfix with `:ccl` and open it with `:copen`
