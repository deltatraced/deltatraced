---
context_type: entry
---

Parent: [lan/2026/main/entry/004 Configuring my gentoo system/004 Configuring my gentoo system](../004%20Configuring%20my%20gentoo%20system.md)

Spawned by: [lan/2026/main/entry/004 Configuring my gentoo system/entry/000 Spawns for Configuring my gentoo system](000%20Spawns%20for%20Configuring%20my%20gentoo%20system.md)

Spawned in: [^spawn-entry-078816](000%20Spawns%20for%20Configuring%20my%20gentoo%20system.md#spawn-entry-078816)

# Journal

2026-07-26 Wk 30 Sun - 20:59 +03:00

Right now my workflow with switching is often `win+s window_name`, but then I have to keep fullscreening that again, so `win+s window_name win+f`.

At first I thought I will modify `sws` so that it fullscreens by default, but I am not yet sure if I always want to be fullscreened. I probably *do* want to make use of my two monitors so that they host more than just 2 windows, so that will eventually be disruptive.

One possible way to deal with this is to have it automatically full screen but only if we are in the tabs layout. In other layouts, I actually do want the spatial configuration, and it shouldn't be automatically disrupted. If it happens that I do want the tabbed layout after all, I will undo the fullscreen with win+f. It is a tradeoff, I believe I will want to switch to a tab full screen more often than to make use of the tabbed layout.

In general personally I prefer not to cycle through more than 3 tabs max, since it ends up being a noticable search.

Spawn [lan/2026/main/entry/004 Configuring my gentoo system/task/000 Modify sway switcher to automatically fullscreen on swap if in tab layout](../task/000%20Modify%20sway%20switcher%20to%20automatically%20fullscreen%20on%20swap%20if%20in%20tab%20layout.md) ^spawn-task-5511da

2026-07-27 Wk 31 Mon - 11:02 +03:00

I kind of like just using `./start_sway.sh` at boot instead of automating it. The login can remain purely at the TTY1 level.

2026-08-04 Wk 32 Tue - 18:04 +03:00

Related to idea [001 Cubical Tabs](../../../../topic/ideas/000%20Ideas/entry/001%20Cubical%20Tabs/001%20Cubical%20Tabs.md),

Currently I'm using tmux with nvim, with the rule that every transition must be a toggle. So

* only two horizontal tmux panes per window,
* only 2 tmux windows per tmux session,
* only 2 vertical splits per neovim session in a given tmux pane.
  * Typically, I open at max two files from the start and use `:bn` to switch between them.
* only 2 tabs per neovim session

This makes everything a toggle. `C-a p` between tmux windows, `C-a k` between tmux panes, `C-w w` between vim splits, and currently `:tabp` between tabs.

It is not exactly like cubical tabs, the idea there is every node is connected to others in a cubical layout. but you can't go from `tmux window 0 upper pane vim right vsplit` to `tmux window 1 upper pane vim right vsplit` for example. You have to change the tmux window from `0` to `1`, and you then have to navigate to `upper pane vim right vsplit` (unless you happen to be there).

But it doesn't require any additional configuration work, and satisfies the property that all navigation is either fuzzy by name (`C-g` to change to tmux session by name, `Win+s` to change to sway application by name) or by toggle so that we do not have to remember any cycling, there's always one "other" we have to remember and assign meaning.

So for example for a given neovim session, I can give the split other the meaning of a reference file or a file I am simultaneously editing.

The tab other can be for reference sections in the of the same file or edit the file in two simultaneous places (for example reading the impl of a function and writing tests for it).

Tmux pane other can just be there for options, I may need to edit 4 files at once for some reason. Tmux tab other can contain at least one tmux pane for terminal, and maybe an extra vim session if we need to edit 6 simultaneous files for some reason.

I use two screens. So I also have a monitor other. I may be interested in programming and taking notes simultaneously. Sometimes I may divide my monitor into two halves via sway. But with sway librewolf tabs for example, I let there be many unconstrained tabs per workspace, those I generally intend to reach by fuzzy switch to name, but I may also do some cycling with some tabs sometimes currently.

The monitor other can be more flexible, since I may want to switch to other workspaces on each monitor, but once I switch to two workspaces on each monitor, then switching back and forth is easy if they are full screened with `Win <-` and `Win ->`.

2026-08-04 Wk 32 Tue - 21:06 +03:00

I should have wrote this for the raw journals so they can have consistent timestamp cards, but oh well:

````vimscript
command! Tims call FnTims()

function FnTims()
        let l:timestamp=system("date \"+%Y-%m-%d\ Wk\ %V\ %a\ -\ %R\ %:z\"")
        execute "normal! i" . l:timestamp . "\<Esc>"
endfunction
````

2026-08-29 Wk 35 Sat - 12:19 +03:00

Could be nice to have more workspace numbers with sway

* https://www.reddit.com/r/swaywm/comments/163nrn1/deleted_by_user/
* $\to$ https://github.com/EllaTheCat/dopamine-2020/blob/master/i3-config.d/cfg03

This person does double digits.

--/ 2026-08-30 Wk 35 Sun - 04:50 +03:00
Disabling this configuration in favor of fuzzy searchable named workspaces as implemented in [001 Add switch by name for sway workspaces](../task/001%20Add%20switch%20by%20name%20for%20sway%20workspaces.md)
--/

2026-08-30 Wk 35 Sun - 01:30 +03:00

Spawn [lan/2026/main/entry/004 Configuring my gentoo system/task/001 Add switch by name for sway workspaces](../task/001%20Add%20switch%20by%20name%20for%20sway%20workspaces.md) ^spawn-task-2dd401
