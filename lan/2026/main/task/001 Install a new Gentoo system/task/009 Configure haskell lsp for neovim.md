---
context_type: task
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/entry/006 Configuring nvim on new gentoo install](../entry/006%20Configuring%20nvim%20on%20new%20gentoo%20install.md)

Spawned in: [^spawn-task-48de34](../entry/006%20Configuring%20nvim%20on%20new%20gentoo%20install.md#spawn-task-48de34)

# Journal

2026-08-23 Wk 34 Sun - 06:40 +03:00

* https://github.com/haskell/lsp
* $\to$ https://github.com/haskell/haskell-language-server
* $\to$ https://haskell-language-server.readthedocs.io/en/latest/installation.html

````sh
ghcup install hls

# Warned to get the latest
ghcup install cabal 3.18.1.0-r0
ghcup install ghc 9.14.1-r0
````

2026-08-23 Wk 34 Sun - 07:25 +03:00

* https://github.com/neovim/nvim-lspconfig

* $\to$ https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md

* $\to$ https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#hls

* https://haskell-language-server.readthedocs.io/en/latest/configuration.html#neovim

````lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init_d/lsp/hls/init.lua
vim.lsp.config('hls', {
    filetypes = { 'haskell', 'lhaskell', 'cabal' }
})

vim.lsp.enable('hls')
````

Spawn [lan/2026/main/task/001 Install a new Gentoo system/task/010 Update moved documentation to haskell docs for neovim hls lsp](010%20Update%20moved%20documentation%20to%20haskell%20docs%20for%20neovim%20hls%20lsp.md) ^spawn-task-8c50e9

2026-08-23 Wk 34 Sun - 07:56 +03:00

https://vi.stackexchange.com/a/43215 getting `vim.ls.log`

````
:lua =require('vim.lsp.log').get_filename()
````

in nvim points for me to \`/home/lan/.local/state/nvim/lsp.log

It doesn't show the individual actions. Might require debugging to be enabled somewhere.

2026-08-23 Wk 34 Sun - 09:22 +03:00

There's also https://github.com/MrcJkb/haskell-tools.nvim

2026-08-23 Wk 34 Sun - 09:23 +03:00

Let's create a temporary project to test the language server with.

I often see projects using `cabal`.

https://cabal.readthedocs.io/en/3.4/getting-started.html

````sh
cabal --version

# out
cabal-install version 3.18.1.0
compiled using version 3.18.1.0 of the Cabal library
````

https://cabal.readthedocs.io/en/3.4/file-format-changelog.html

````sh
# in /home/lan/src/cloned/local/lan/scratch-hs
cabal init --cabal-version=3.4 --license=MIT -p scratch-hs
````

````sh
tree

# out
.
├── app
│   └── Main.hs
├── CHANGELOG.md
├── LICENSE
├── README.md
└── scratch-hs.cabal

2 directories, 5 files
````

We can use `cabal build` and `cabal run`. `cabal build :scratch-hs` didn't work for me for some reason.

I can use `K` to read the documentation of `putStrLn`, and `C-x C-o` to autocomplete `putStrLn`., and `C-w d` to read diagnostics.

`C-]` still expects a tag file though. Although it actually works for a function I define like `double` and `main`.

````haskell
-- in /home/lan/src/cloned/local/lan/scratch-hs/app/Main.hs
module Main (main) where

double :: Int -> Int
double x = x * 2

main :: IO ()
main = putStrLn ("Double it: " ++ show (double 4))
````

````sh
# in /home/lan/src/cloned/local/lan/scratch-hs
cabal run

# out
Double it: 8
````

Also can't follow to `show`.

It says it's defined in `GHC-Internal.Show`.

2026-08-23 Wk 34 Sun - 09:48 +03:00

So this confirms that we have lsp as we expect. Why is it not working in `/home/lan/src/cloned/gh/agda/cornelis`?

That also has a `*.cabal` file.

`K` seems to work on `main` after I try to force an edit of that file.

It still won't follow with `C-]` to functions outside the current project like `neovim`. but `K` works on it.

Oh wait I didn't build it. Let's build it and see.

````sh
# in /home/lan/src/cloned/gh/agda/cornelis
cabal build
````

That ends up in a compiler error

````
/home/lan/src/cloned/gh/agda/cornelis/src/Cornelis/Utils.hs:68:51: error: [GHC-83865]
68 |   for wins $ \w -> fmap (w, ) $ window_get_buffer w
   |                                                   ^
````

Use `stack build`... Still.

`cabal clean; stack clean; stack build`... Still.

`COMPATIBILITY.md` says it's difficult to stay up to date with Agda versions so you may need to downgrade.
