---
context_type: task
status: todo
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/006 Configuring nvim on new gentoo install]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/006 Configuring nvim on new gentoo install#^spawn-task-48de34|^spawn-task-48de34]]

# Journal

2026-08-23 Wk 34 Sun - 06:40 +03:00

- https://github.com/haskell/lsp
- $\to$ https://github.com/haskell/haskell-language-server
- $\to$ https://haskell-language-server.readthedocs.io/en/latest/installation.html

```sh
ghcup install hls

# Warned to get the latest
ghcup install cabal 3.18.1.0-r0
ghcup install ghc 9.14.1-r0
```

2026-08-23 Wk 34 Sun - 07:25 +03:00

- https://github.com/neovim/nvim-lspconfig
- $\to$ https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md
- $\to$ https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#hls

- https://haskell-language-server.readthedocs.io/en/latest/configuration.html#neovim

```lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/lsp/hls/init.lua
vim.lsp.config('hls', {
    filetypes = { 'haskell', 'lhaskell', 'cabal' }
})

vim.lsp.enable('hls')
```

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/task/010 Update moved documentation to haskell docs for neovim hls lsp]] ^spawn-task-8c50e9

2026-08-23 Wk 34 Sun - 07:56 +03:00

https://vi.stackexchange.com/a/43215 getting `vim.ls.log`

```
:lua =require('vim.lsp.log').get_filename()
```

in nvim points for me to `/home/lan/.local/state/nvim/lsp.log`