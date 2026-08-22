---
context_type: task
status: todo
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/012 Configuring Agda]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/012 Configuring Agda#^spawn-task-afbd97|^spawn-task-afbd97]]

# Journal

2026-08-22 Wk 34 Sat - 21:26 +03:00

Foking `vim-agda`. It hardcodes the agda executable, so we couldn't even try to use mikan there.

https://github.com/LanHikari22/vim-agda

https://github.com/msuperdock/vim-agda/issues/6

```sh
# in /home/lan/src/forked/gh/LanHikari22/msuperdock/branches
git clone git@github.com:LanHikari22/vim-agda.git
mv vim-agda vim-agda@allow-configurable-agda-exec
cp -r vim-agda@allow-configurable-agda-exec vim-agda@fork-main

# in /home/lan/src/forked/gh/LanHikari22/msuperdock/branches/vim-agda@allow-configurable-agda-exec
git checkout -m allow-configurable-agda-exec
```

2026-08-22 Wk 34 Sat - 21:42 +03:00

Previously I tried to override this in configuration, but it uses local functions, and it's better fixed in the plugin itself:

```lua
-- https://github.com/msuperdock/vim-agda/blob/main/autoload/agda.vim
-- Reconfiguring to override the executable agda job
vim.cmd([[
    let g:agda_debug = 0
    let g:agda_args = []

    try
        let g:agda_job = jobstart(['agda', '--interaction-json'] + g:agda_args
            \ , {'on_stdout': function('s:handle_event')}) " <-- I do not have s:handle_event
    catch /E475/
        echom 'Agda executable not found.'
    endtry
]])
```

2026-08-22 Wk 34 Sat - 21:33 +03:00

```vimscript
" in /home/lan/src/forked/gh/LanHikari22/msuperdock/branches/vim-agda@allow-configurable-agda-exec/ftplugin/agda.vim
if !exists('g:agda_executable')
    let g:agda_executable = 'agda'
endif
```

```diff
# in /home/lan/src/forked/gh/LanHikari22/msuperdock/branches/vim-agda@allow-configurable-agda-exec/autoload/agda.vim
-let g:agda_job = jobstart(['agda', '--interactioindicating it is using the mikan executable which is aware of `Type`.n-json'] + g:agda_args
+let g:agda_job = jobstart([g:agda_executable, '--interaction-json'] + g:agda_args
```

2026-08-22 Wk 34 Sat - 21:39 +03:00

Okay let's test this locally then.

```lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/plugin/init.lua
--vim.pack.add{ { src = 'https://github.com/msuperdock/vim-agda' }, }
vim.pack.add{ { src = '/home/lan/src/forked/gh/LanHikari22/msuperdock/branches/vim-agda@allow-configurable-agda-exec' }, }
```

There's probably a better way to clear plugins.

```sh
rm -rf ~/.local/share/nvim/site/pack/core/opt/vim-agda/
rm -f ~/.config/nvim/nvim-pack-lock.json
```

To reload the local plugin:

```sh
rm -rf ~/.local/share/nvim/site/pack/core/opt/vim-agda\@allow-configurable-agda-exec/
rm -f ~/.config/nvim/nvim-pack-lock.json
```

The content of `~/.local/share/nvim/site/pack/core/opt/vim-agda\@allow-configurable-agda-exec/autoload/agda.vim` does not reflect the changes I have. But it does after commit.

```sh
# in /home/lan/src/forked/gh/LanHikari22/msuperdock/branches/vim-agda@allow-configurable-agda-exec
git commit # out { [allow-configurable-agda-exec 43be77c] add g:agda_executable }
```

2026-08-22 Wk 34 Sat - 22:05 +03:00

Yup it seems responsive now. Here is the test file:

```haskell
-- in ~/a.agda
open import Agda.Primitive using (
    LevelUniv; 
    Level) renaming (
    lzero to ℓ-zero;
    lsuc to ℓ-suc;
    _⊔_ to ℓ-max)

open import Agda.Primitive.Cubical using (
    I; 
    i0; 
    i1; 
    Partial) renaming (
        primIMin to infixr 20 _∧_;
        primIMax to infixr 20 _∨_;
        primINeg to infix 30 ~_)

∂ : I → I
∂ i = i ∨ (~ i)

-- Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial (~ i ∨ ∂ j ∨ ~ k) A
Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
Repro i j k (i = i0) = {!!}
```

When configuring the plugin to use `agda`,

```lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/gh/msuperdock/vim-agda/init.lua
vim.cmd([[
    let g:agda_executable = "agda"
]])
```

and we run `:call agda#load()` on `~/a.agda` we get

```
/home/lan/a.agda:21.28-32: error: [NotInScope]
Not in scope:
  Type at /home/lan/a.agda:21.28-32
when scope checking Type
```

When configuring the plugin to use `mikan`,

```lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/gh/msuperdock/vim-agda/init.lua
vim.cmd([[
    let g:agda_executable = "mikan"
]])
```

and we run `:call agda#load()` on `~/a.agda` we get

```
/home/lan/a.agda:22.1-28: error: [UnequalTerms]
The terms
  ~ i ∨ (j ∨ ~ j) ∨ ~ k
and
  ~ i
are not equal at type I
when checking the definition of Repro
```

indicating it is using the mikan executable which is aware of `Type`.

When configuring the plugin to use bogus `echo`,

```lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/gh/msuperdock/vim-agda/init.lua
vim.cmd([[
    let g:agda_executable = "echo"
]])
```

and we run `:call agda#load()` on `~/a.agda` we get

```
Loading Agda.
```

Run it again we get

```
Loading Agda (command ignored).
```

So it has some mechanism of indicating the command is invalid. If we set `g:agda_executable` to `""` we instead get `Agda executable not found.`.
