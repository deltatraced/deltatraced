---
context_type: entry
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System#^spawn-entry-3fc3d7|^spawn-entry-3fc3d7]]

# Journal

2026-08-02 Wk 31 Sun - 15:39 +03:00

https://docs.virtorg.org/neovim/configuration/

From `nvim > :checkhealth`,

```
Configuration
- ⚠️ WARNING Missing user config file: /home/lan/.config/nvim/init.lua
  - ADVICE:
    - :help nvim-from-vim
```

https://docs.virtorg.org/neovim/deep_dive/

https://neovim.io/doc/user/lua-guide/

https://stackoverflow.com/questions/75665675/how-to-properly-source-use-lua-file-in-init-vim-config-for-neovim
- We can use `init.vim` too, and with this we can use `source`

https://stackoverflow.com/questions/4976776/how-to-get-path-to-the-current-vimscript-being-executed

It seems doing `source "/path/to/init.lua"` doesn't work through the command prompt `:source`, but `source /path/to/init.lua` does.

Fixing that, we can tell that our `init.lua` runs now, we get a print.

vim options like `hlsearch` can be set via `vim.opt.{option}` (ex: `vim.option.hlsearch = false`)

2026-08-03 Wk 32 Mon - 15:14 +03:00

Spawn [[000 require neovim lua scripts recursively down a directory tree anywhere]] ^spawn-howto-d871a5

2026-08-03 Wk 32 Mon - 14:54 +03:00

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/entry/009 Configuring nvim rust_analyzer lsp on new gentoo install]] ^spawn-entry-0ac83f

2026-08-04 Wk 32 Tue - 14:48 +03:00

Let's get sneak motion: https://neovimcraft.com/plugin/justinmk/vim-sneak/ https://github.com/justinmk/vim-sneak

```sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/plugin/init.lua
vim.pack.add{ { src = 'https://github.com/justinmk/vim-sneak' }, }
```

2026-08-06 Wk 32 Thu - 01:59 +03:00

We can run headless commands:

https://www.reddit.com/r/neovim/comments/79cs6i/running_commands_in_neovim_from_the_commandline/

```sh
nvim --headless +":lua print(\"hi\n\")" +q

# out
hi
```

Then we can do `nvim --headless +":nmap" +q 2>&1 | less` to inspect keybindings

https://www.reddit.com/r/neovim/comments/165tjxa/how_to_get_all_keybindings/
- $\to$ https://www.reddit.com/r/neovim/comments/150fb8d/comment/js55cje/?utm_source=share&utm_medium=web2x&context=3

```
:h nvim_get_kemap
```

https://www.reddit.com/r/neovim/comments/15qyenb/remove_prefixed_dot_when_using_cxco_for_completion/

I want to find this `<C-X><C-O>` autocompletion mode. In neovim it says `^X mode (^]^D^E^F^I^K^L^N^O^P^Rs^U^V^Y)`

2026-08-23 Wk 34 Sun - 06:39 +03:00

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/task/009 Configure haskell lsp for neovim]] ^spawn-task-48de34

