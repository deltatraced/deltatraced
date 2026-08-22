---
context_type: entry
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System#^spawn-entry-caaa8f|^spawn-entry-caaa8f]]

# Journal

2026-08-22 Wk 34 Sat - 16:53 +03:00

Vim-mode in emacs: https://github.com/emacs-evil/evil

So the settings for this should go to some emacs init file.

- https://www.gnu.org/software/emacs/documentation.html
- $\to$ https://www.gnu.org/software/emacs/manual/html_node/emacs/index.html
- $\to$ https://www.gnu.org/software/emacs/manual/html_node/emacs/Init-File.html

Seems emacs looks in many places for configuration, let's use `~/.config/emacs/init.el`. The primary thing is we want it just point to configuration we have in our dotfiles repo.

2026-08-22 Wk 34 Sat - 17:35 +03:00

Hmm. when loading this manually in the emacs editor I am able to access vim mode: `(load "/home/lan/src/cloned/cb/lan22h/dotfiles/etc/emacs/init.el")`

but it doesn't seem to load automatically on start with that being in `~/.config/emacs/init.el`.

Trying to put it instead in `~/.emacs.d/init.el`. Still the same. We can use the command `(print "something")`

https://stackoverflow.com/a/864939 suggests strace

`strace -o ~/emacs.log -e open emacs`. But I don't get much information through this even in su.

But there is a file `~/.emacs` Deleting it and putting the load in the `~/.emacs.el` which should be the first file it checks. Now it loads it on start.


https://stackoverflow.com/a/21767679 A method to load files recursively in a directory


https://stackoverflow.com/a/4088981 Getting the current script file's directory

https://stackoverflow.com/a/3964815 Concating the path with a relative path. Warns against manual "/" entry.

2026-08-22 Wk 34 Sat - 18:23 +03:00

Okay we're now able to recursively load configuration in `init_d` similar to how we did in with nvim. Basically to have an `init.el` in each directory be responsible for loading the ones under it.

And now we have the vim configuration in `/home/lan/src/cloned/cb/lan22h/dotfiles/etc/emacs/init_d/gh/emacs-evil/evil/init.el` confirmed to be running.

2026-08-22 Wk 34 Sat - 18:28 +03:00

Now that emacs is recursively configurable we need configuration for https://codeberg.org/1lab/mikan and https://agda.readthedocs.io/en/v2.6.2.2/tools/emacs-mode.html

https://agda.readthedocs.io/en/v2.6.2.2/getting-started/installation.html#running-the-agda-mode-program

[[002 Quick new Installs for Gentoo System#Agda]]
