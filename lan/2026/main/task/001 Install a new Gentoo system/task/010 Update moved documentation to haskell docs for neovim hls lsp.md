---
context_type: task
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/task/009 Configure haskell lsp for neovim](009%20Configure%20haskell%20lsp%20for%20neovim.md)

Spawned in: [^spawn-task-8c50e9](009%20Configure%20haskell%20lsp%20for%20neovim.md#spawn-task-8c50e9)

# Journal

2026-08-23 Wk 34 Sun - 07:35 +03:00

* https://haskell-language-server.readthedocs.io/en/latest/configuration.html#neovim
* points to https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#hls

But the latter says the configuration was moved to `configs.md`.

The correct new path should be https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#hls.

2026-08-23 Wk 34 Sun - 07:39 +03:00

Now how to contribute to this?

* There is an `Edit on Github` in this page: https://docs.readthedocs.com/platform/stable/changelog.html
* $\to$ https://github.com/readthedocs/readthedocs.org/blob/main/docs/user/changelog.rst

But I couldn't find anything about `haskell-language-server` there.

Searching the repository page for `haskell-language-server` I was able to find this: https://github.com/haskell/haskell-language-server/blob/master/.readthedocs.yaml

It seems it configures this `.readthedocs` documentation page service.

The page of interest we want is https://github.com/haskell/haskell-language-server/blob/master/docs/configuration.md#neovim

(permalink https://github.com/haskell/haskell-language-server/blob/a4cfaa80ca94beded6f01547a161b37be7b33558/docs/configuration.md#neovim)

Specifically the line

````md
Includes a basic [`hls` configuration](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#hls).
````

Let's look for similar documentation PRs for patterns:

* https://github.com/haskell/haskell-language-server/pull/4997
* https://github.com/haskell/haskell-language-server/pull/4824

We'll follow the pattern `docs: ...` for the commit and PR title.

Most of these just create the PR directly instead of creating an additional issue. Likely because it is a simple change, so let's follow this convention.

````sh
# in /home/lan/src/forked/gh/LanHikari22/haskell/branches
git clone git@github.com:LanHikari22/haskell-language-server.git
mv haskell-language-server/ haskell-language-server@docs-update-moved-nvim-lspconfig-page
````

````diff
# in /home/lan/src/forked/gh/LanHikari22/haskell/branches/haskell-language-server@docs-update-moved-nvim-lspconfig-page/docs/configuration.md
-  - Includes a basic [`hls` configuration](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#hls).
+  - Includes a basic [`hls` configuration](https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#hls).
````

````sh
# in /home/lan/src/forked/gh/LanHikari22/haskell/branches/haskell-language-server@docs-update-moved-nvim-lspconfig-page
git commit # out (relevant) { [master a15da1bd] docs: updated moved nvim-lspconfig page }
````

2026-08-23 Wk 34 Sun - 08:37 +03:00

Here's the PR: https://github.com/haskell/haskell-language-server/pull/5051.
