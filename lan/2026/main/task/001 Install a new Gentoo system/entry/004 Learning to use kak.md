---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/task/004 Setup a new code editor for new gentoo](../task/004%20Setup%20a%20new%20code%20editor%20for%20new%20gentoo.md)

Spawned in: [^spawn-entry-717e09](../task/004%20Setup%20a%20new%20code%20editor%20for%20new%20gentoo.md#spawn-entry-717e09)

# Journal

2026-08-02 Wk 31 Sun - 13:43 +03:00

Recently installed kakoune, coming from primarily vim and vim-like editors. At least I have hjkl, but my dd is gone!

`gg` is still around, but no more `G`. Now we get a more consistent extension like `e`: `ge` for buffer end. There's some clippy TUI window that explains the keys.

Still got forward search `/`, but no more `?` for backwards search. Still got `n` for next search result but no more `N`. `?` seems to do something else now. It can let you extend a selection.

Still have `u`ndo, but for redo it's `U`.

`i`nsert mode. `Esc` back to normal? mode. `v` is now for some view actions like scrolling rather than visual mode.

Oh I can do `xd` instead of `dd` to delete a line. Yay.

You can just use `d` to start deleting characters in normal? mode so that's fun.

We still have `r` for replace character, no more `R` for a replace mode.

We still have `o` for next line editing. and `O` for previous line.

`y` on its own now yanks to some register `"`.  And I can do `"ay` to yank to register a. No more `yy` which in vim yanks a line.

`$` and `0` no longer take you to the end/start of a line. But now this became part of `g`! `gh`/`gl`.

I don't have to keep pressing `v` to scroll, using `V` makes it sticky.

`G` instead of `g` selects, so we can do `gg` then `Ged` to delete all file content. Also, to compliment `gh/gl` for lines, you can use `gk/gj` for buffer.

`f` now doesn't navigate to a forward character, it selects to it. Not sure on `F`, but not backwards go to char. We still have `t` which stops just before, rather than at. We can use `c` to delete also, not just `d`, and it retains switching into insert mode rather than staying in normal? mode.

`w` stilll works for next word (also `b` for prev word and `e` for end of word), almost didn't notice. I guess I use them a lot.

Used to be able to do `ce` to delete word in vim... Now we can do `ec` (and `wc`)! Can also be used in the middle of the word for a partial selection. Just navigating selects words though, so I probably can just use `c` a lot of the time.

Hmm I am able to paste from a register with `p`

`%` still works for going between parentheses, and now it selects.
