---
context_type: entry
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System#^spawn-entry-9b07ea|^spawn-entry-9b07ea]]

# Journal

2026-08-22 Wk 34 Sat - 21:16 +03:00

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/task/008 Fork msuperdock vim-agda and allow agda executable to be configurable]] ^spawn-task-afbd97

2026-08-22 Wk 34 Sat - 22:34 +03:00

Okay, until this change is merged, we will be getting it from my own fork's `fork-main` branch:

```sh
# in /home/lan/src/forked/gh/LanHikari22/msuperdock/branches/vim-agda@fork-main
git checkout -b fork-main
git pull origin allow-configurable-agda-exec
git push origin fork-main
```

Doing 

```lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/plugin/init.lua
vim.pack.add{ { src = 'https://github.com/LanHikari22/vim-agda/tree/fork-main' }, }
```

causes a vim.pack error:

```
/usr/share/nvim/runtime/lua/vim/pack.lua:250: fatal: repository 'https://github.com/LanHikari22/vim-agda/tree/fork-main/' not found
```

```sh
rm -rf ~/.local/share/nvim/site/pack/core/opt/vim-agda
rm -rf ~/.local/share/nvim/site/pack/core/opt/vim-agda\@allow-configurable-agda-exec/
rm -f ~/.config/nvim/nvim-pack-lock.json
```

```lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/plugin/init.lua
--vim.pack.add{ { src = 'https://github.com/msuperdock/vim-agda' }, }
vim.pack.add{ { src = 'https://github.com/LanHikari22/vim-agda', version = 'fork-main' }, }
```

2026-08-23 Wk 34 Sun - 06:21 +03:00

There is also https://github.com/agda/cornelis for neovim we can setup which seems more maintained.

```sh
export REPO=agda/cornelis && git clone git@github.com:$REPO ~/src/cloned/gh/$REPO
cd ~/src/cloned/gh/$REPO
```

- [[006 Configuring nvim on new gentoo install]]
- $\to$ [[009 Configure haskell lsp for neovim]]