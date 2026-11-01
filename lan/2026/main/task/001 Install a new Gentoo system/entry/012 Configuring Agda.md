---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System](002%20Quick%20new%20Installs%20for%20Gentoo%20System.md)

Spawned in: [^spawn-entry-9b07ea](002%20Quick%20new%20Installs%20for%20Gentoo%20System.md#spawn-entry-9b07ea)

# Journal

2026-08-22 Wk 34 Sat - 21:16 +03:00

Spawn [lan/2026/main/task/001 Install a new Gentoo system/task/008 Fork msuperdock vim-agda and allow agda executable to be configurable](../task/008%20Fork%20msuperdock%20vim-agda%20and%20allow%20agda%20executable%20to%20be%20configurable.md) ^spawn-task-afbd97

2026-08-22 Wk 34 Sat - 22:34 +03:00

Okay, until this change is merged, we will be getting it from my own fork's `fork-main` branch:

````sh
# in /home/lan/src/forked/gh/LanHikari22/msuperdock/branches/vim-agda@fork-main
git checkout -b fork-main
git pull origin allow-configurable-agda-exec
git push origin fork-main
````

Doing

````lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/plugin/init.lua
vim.pack.add{ { src = 'https://github.com/LanHikari22/vim-agda/tree/fork-main' }, }
````

causes a vim.pack error:

````
/usr/share/nvim/runtime/lua/vim/pack.lua:250: fatal: repository 'https://github.com/LanHikari22/vim-agda/tree/fork-main/' not found
````

````sh
rm -rf ~/.local/share/nvim/site/pack/core/opt/vim-agda
rm -rf ~/.local/share/nvim/site/pack/core/opt/vim-agda\@allow-configurable-agda-exec/
rm -f ~/.config/nvim/nvim-pack-lock.json
````

````lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/plugin/init.lua
--vim.pack.add{ { src = 'https://github.com/msuperdock/vim-agda' }, }
vim.pack.add{ { src = 'https://github.com/LanHikari22/vim-agda', version = 'fork-main' }, }
````

2026-08-23 Wk 34 Sun - 06:21 +03:00

There is also https://github.com/agda/cornelis for neovim we can setup which seems more maintained.

````sh
export REPO=agda/cornelis && git clone git@github.com:$REPO ~/src/cloned/gh/$REPO
cd ~/src/cloned/gh/$REPO
````

* [006 Configuring nvim on new gentoo install](006%20Configuring%20nvim%20on%20new%20gentoo%20install.md)
* $\to$ [009 Configure haskell lsp for neovim](../task/009%20Configure%20haskell%20lsp%20for%20neovim.md)

It doesn't build.

2026-08-23 Wk 34 Sun - 11:47 +03:00

How do I get emacs agda mode to recognize `.lagda.md` files?

Some work in [011 Configuring emacs in main gentoo system](011%20Configuring%20emacs%20in%20main%20gentoo%20system.md)

* https://codeberg.org/1lab/mikan
* https://agda.readthedocs.io/en/v2.6.2.2/tools/emacs-mode.html

2026-08-23 Wk 34 Sun - 12:02 +03:00

https://github.com/agda/agda/issues/2837

This targets lack of support for `.lagda.md`

https://github.com/agda/agda/issues/2837#issuecomment-473282957 mentions

````lisp
(add-to-list 'auto-mode-alist '("\\.lagda.md\\'" . agda2-mode))
````

It works!

Maybe this should be in the docs? I'll just put a mention in chat, as both the mikan and agda teams could pick up on this.

[\#general > agda-mode doesn't recognize .lagda @ 💬](https://agda.zulipchat.com/#narrow/channel/238741-general/topic/agda-mode.20doesn.27t.20recognize.20.2Elagda/near/618387835)
