---
context_type: entry
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System#^spawn-entry-72fffc|^spawn-entry-72fffc]]

# Journal

2026-08-06 Wk 32 Thu - 18:26 +03:00

https://wiki.gentoo.org/wiki/Sway#Switching_Keyboard_Layouts

https://wiki.gentoo.org/wiki/Keyboard_layout_switching

https://wiki.gentoo.org/wiki/Localization/Guide

Find codes with `find /usr/share/keymaps/i386/ | grep 'fr-'`

```sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/sway/config {
	input type:keyboard {
		xkb_layout "us,ar"
		xkb_options "grp:alt_shift_toggle"
	}
# }
```

2026-08-22 Wk 34 Sat - 23:29 +03:00

https://fcitx-im.org/wiki/Install_and_Configure

We can do

```
sway input type:keyboard xkb_layout us
sway input type:keyboard xkb_layout fn
sway input type:keyboard xkb_layout eg
```

to switch keyboard layouts manually also.

2026-08-22 Wk 34 Sat - 23:58 +03:00

https://github.com/swaywm/sway/wiki#keyboard-layout

2026-08-23 Wk 34 Sun - 00:59 +03:00

Okay I fixed my sway config, right now I have that it needs to be copied directly rather than sourcing directly from the dotfiles repo. So this is handled by sway's `install.sh`.

2026-08-23 Wk 34 Sun - 01:03 +03:00

Now that I can switch keyboard layouts with alt+shift, I need to add pinyin. We need IME keyboard support.

- https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland
- $\to$ https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland#Sway

https://wiki.gentoo.org/wiki/Fcitx

[[002 Quick new Installs for Gentoo System#Fcitx]]

2026-08-23 Wk 34 Sun - 01:45 +03:00

https://www.reddit.com/r/swaywm/comments/i6qlos/how_do_i_use_an_ime_with_sway/

A user by the name `SpaceshipOperations` there has some guidelines. For example their configuration mentions setting 

They also set `GTK_IM_MODULE=fcitx` which also appears here: https://wiki.archlinux.org/title/Fcitx5

`fcitx5-configtool` can be used to configure the languages of interest. I added pinyin and Wubi, and English.

2026-08-23 Wk 34 Sun - 02:47 +03:00

It works! Although we might have to look into running it as a service. I can use Ctrl+Space to switch between languages configured in `fcitx5-configtool`. There is a weird thing were pinyin types arabic and doesn't quite work, if my sway layout is in arabic. Also, the Arabic language setting types English. But it isn't a high priority issue, as I can get arabic via the sway keyboard layout, and pinyin and wubi through fcitx5.