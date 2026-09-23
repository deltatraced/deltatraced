---
context_type: issue
status: wontdo
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/entry/011 Configuring emacs in main gentoo system](../entry/011%20Configuring%20emacs%20in%20main%20gentoo%20system.md)

Spawned in: [^spawn-issue-7b656d](../entry/011%20Configuring%20emacs%20in%20main%20gentoo%20system.md#spawn-issue-7b656d)

# Journal

2026-09-20 Wk 38 Sun - 22:33 +03:00

````
user-error: Customize ‘evil-undo-system’ for redo functionality.
````

emacs gives this error in vim mode when I try to redo (undo undo) with C-R.

https://github.com/syl20bnr/spacemacs/issues/14036#issuecomment-809102934 mentions `(evil-set-undo-system 'undo-tree)`

https://github.com/syl20bnr/spacemacs/issues/14036#issuecomment-809102934 mentions `undo` in addition to `undo-tree`

Added `(evil-set-undo-system 'undo)` to `/home/lan/src/cloned/cb/lan22h/dotfiles/etc/emacs/init_d/gh/emacs-evil/evil/init.el` before enabling `Evil`.

I got an error putting it before `(require 'evil)`. But not with

````ls
;; Enable Evil
(require 'evil)

(evil-set-undo-system 'undo-tree)

(evil-mode 1)
````

This doesn't cause errors, but `'undo` does. But when we do this, then even normal undo functionality changes how it works, so we don't want that.

There's also `'undo-fu` I found in https://codeberg.org/ideasman42/emacs-undo-fu which `(evil-set-undo-system 'undo-fu)` accepts, but it also changes the normal undo functionality.

Note that `C-R` does seem to allow redo on some sort of toggle without configuring anything. So I could do `C-R u` and get back what I wrote. So maybe this is enough.

For now, we will stick with that.

wontdo
