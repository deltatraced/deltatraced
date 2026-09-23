---
context_type: task
status: done
---

Parent: [lan/2026/main/entry/004 Configuring my gentoo system/004 Configuring my gentoo system](../004%20Configuring%20my%20gentoo%20system.md)

Spawned by: [lan/2026/main/entry/004 Configuring my gentoo system/entry/001 Gentoo Usage Thought Stream](../entry/001%20Gentoo%20Usage%20Thought%20Stream.md)

Spawned in: [^spawn-task-2dd401](../entry/001%20Gentoo%20Usage%20Thought%20Stream.md#spawn-task-2dd401)

# Journal

2026-08-30 Wk 35 Sun - 01:47 +03:00

With the double digit workspace, I encoded 1x to the left monitor and 2x to the right. Possibly could also have an "alternative" for each by going odds/events: 1x has alternative 3x, and 2x has alternative 4x.

I already setup switching to windows by name, for example there can be *many* browsing subcontexts. But for the broad browsing activity, I could switch to it by number.

We could also play with a similar semantics to what I do in tmux which worked fairly well for me:

1. Have a binding to create a named sway workspace
1. Have another binding to fuzzy switch to it by name
1. Have another binding that allows you to toggle between the last opened workspaces.

This will be more keys than the numbers, but much more memorable.

Although after a while I usually memorize what numbers are for what, and numbers are fewer keystrokes in the end.

https://davemq.github.io/2026/02/24/rofi-workspace-switcher-sway.html

This handles the fuzzy switching (2) between workspaces. For (1), apparently we only need to do `swaymsg workspace {name}`.

11https://www.reddit.com/r/swaywm/comments/18vaxaw/keybinding_to_rename_current_workspace_to_user/

mentions using `wofi` to prompt for a name.

https://www.reddit.com/r/swaywm/comments/rnv0su/can_you_do_an_in_place_reload_of_sway_like_in_i3wm/

mentions that `mod+shift+c` reloads config. Though we could have bad config, will we be able to get out of that?

2026-08-30 Wk 35 Sun - 02:42 +03:00

[002 Quick new Installs for Gentoo System > Wofi](../../../task/001%20Install%20a%20new%20Gentoo%20system/entry/002%20Quick%20new%20Installs%20for%20Gentoo%20System.md#wofi)

`wofi --dmenu` is similar visually to our tmux-sessionist. While our current `sws` is a top bar, we may need to sometimes see multiple options, and it's better for switching to be obvious, center of screen. (`sws`: [003 sws switcher can after a long time fail under sway but keeps working if launched manually](../../../task/001%20Install%20a%20new%20Gentoo%20system/issue/003%20sws%20switcher%20can%20after%20a%20long%20time%20fail%20under%20sway%20but%20keeps%20working%20if%20launched%20manually.md))

2026-08-30 Wk 35 Sun - 03:25 +03:00

Actually, `sws` already handles workspace *and* application name fuzzy open! So we only need to add a binding to create named workspaces!

2026-08-30 Wk 35 Sun - 03:29 +03:00

`wmenu` was able to always show up, even in full screen though. `wofi` currently does not.

https://www.reddit.com/r/swaywm/comments/qlhbbk/miniguide_show_wofi_on_full_screen_mode/

We can use `wofi --dmenu -D layer=overlay`

also apparently `-G` for dark variant per https://man.archlinux.org/man/wofi.1.en.

2026-08-30 Wk 35 Sun - 03:56 +03:00

Even without a key binding, we can just do `$mod+d wsnew` now with just an intuitively named command in `~/.local/bin`. I added also `wsrename`.

2026-08-30 Wk 35 Sun - 04:16 +03:00

We can make `sws` full screen on switch. Sometimes that's not what we want, but it seems often it is.

https://man.archlinux.org/man/sway.5.en

`sway fullscreen enable`

2026-08-30 Wk 35 Sun - 04:22 +03:00

For switching back and forth between the workspaces

https://www.reddit.com/r/swaywm/comments/qv548y/switch_between_current_and_last_workspace/

````
# Toggle workspaces
bindsym $mod+grave workspace back_and_forth
````

They also have

````
# Switch to prev/next workspace on current output
bindsym $mod+n workspace next_on_output
bindsym $mod+p workspace prev_on_output
# Switch to prev/next workspace on all outputs
bindsym $mod+Shift+n workspace next
bindsym $mod+Shift+p workspace prev
````

2026-08-30 Wk 35 Sun - 03:58 +03:00

`wofi --dmenu` doesn't really behave like `fzf`. It could be good to search for the workspace name and then some differentiating part of the window name.

`man 5 wofi` shows that there's a `matching` option. Takes `contains`, `multi-contains`, and `fuzzy`.

Using `multi-contains` instead of contains lets us search different words!

2026-08-30 Wk 35 Sun - 04:42 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles
git commit # out { [main 2092fb2] sway: redirect grim screenshots and setup named workspace switching }
````

OK
