---
parent: "[[000 Getting Started for LCR]]"
spawned_by: "[[000 Compile cubical agda to an executable using ctqs sources]]"
context_type: task
status: done
---

Parent: [[000 Getting Started for LCR]]

Spawned by: [[000 Compile cubical agda to an executable using ctqs sources]]

Spawned in: [[000 Compile cubical agda to an executable using ctqs sources#^spawn-task-207da8|^spawn-task-207da8]]

# 1 Journal

2026-05-11 Wk 20 Mon - 20:18 +03:00

In vscode, we are able to use unicode-mode to write mathematical text with Agda. We wanna do this in vim too.

[(1) gh derekelkins/agda-vim](https://github.com/derekelkins/agda-vim). [(2) wiki.portal.chalmers.se Vim Editing](https://wiki.portal.chalmers.se/agda/pmwiki.php?n=Main.VIMEditing).

`utf8.vim` in `(2)` is not accessible. Neither is `unicode-keys.vim`. But `(1)` contains an `autoload/agda.vim`

There are nvim options too [(3) gh ashinkarov/nvim-agda](https://github.com/ashinkarov/nvim-agda), [(4) Isti115/agda.nvim](https://github.com/Isti115/agda.nvim)

Let's try to install `(1)` since `(3)` and `(4)` are in nvim.

It said we can copy the structure in, maybe like so:

```
# in /home/lan/.vim
git clone https://github.com/derekelkins/agda-vim.git
mv agda-vim/agda-utf8.vim .
mv agda-vim/agda.py .
mv agda-vim/autoload/agda.vim autoload/
mv agda-vim/ftdetect .
mv agda-vim/ftplugin .
mv agda-vim/syntax .
rm -rf agda-vim
```

but this would not be very tidy. Let's install [gh tpope/vim-pathogen](https://github.com/tpope/vim-pathogen):

```sh
mkdir -p ~/.vim/autoload ~/.vim/bundle && \
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```

Then add this to `~/.vimrc`:

```vimrc
execute pathogen#infect()
```

Now we can install it via pathogen:

```sh
# in /home/lan/.vim/bundle
git clone https://github.com/derekelkins/agda-vim.git
```

Awesome! We have syntax highlighting now, and we're able to do `\forall` for example.

`:echo b:did_ftplugin` gives `1` in vim, so we know [ftplugin.vim](https://github.com/derekelkins/agda-vim/blob/master/ftplugin/agda.vim) loaded. ~~But we don't seem to have access to functions like `AgdaLoad`.~~

We can also see if [gh neoclide/coc.nvim](https://github.com/neoclide/coc.nvim) offers an extension in [wiki register-custom-language-servers](https://github.com/neoclide/coc.nvim/wiki/Language-servers#register-custom-language-servers). It is not listed. It makes sense for the more advanced hole-based features etc, but I figured they might support at least the usual pop-up type display for example.

So we are able to call `:call AgdaLoad(v:false)`, but not `:call AgdaVersion(v:false)`

2026-08-30 Wk 35 Sun - 04:44 +03:00

Closed by [[011 Configuring emacs in main gentoo system]]

Other effort is in [[012 Configuring Agda]]