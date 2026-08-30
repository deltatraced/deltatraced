---
context_type: issue
status: todo
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system#^spawn-issue-285bb1|^spawn-issue-285bb1]]

Issues: [[003 Issues for Install a new Gentoo system]]

# Journal

2026-08-01 Wk 31 Sat - 20:20 +03:00

We select the thing to switch to then get an error in the sway logs. This does not happen initially.

Do `tail -f ~/sway_logs.logs`. Usually we get

```json
[
  {
    "success": false,
    "parse_error": true,
    "error": "Unknown\/invalid command 'c'"
  }
]
```

*error (1)*

But we can initially get

```json
[
  {
    "success": false,
    "parse_error": true,
    "error": "The value for 'con_id' should be '__focused__' or numeric"
  }
]
```

*error (2)*

on wrong input.

We confirm on wrong input we also get this via `sws` directly from terminal: `Error: The value for 'con_id' should be '__focused__' or numeric`

But  with `sws` directly from terminal rather than through sway, we are able to swtich with valid input rather than getting `error (1)`.

2026-08-02 Wk 31 Sun - 22:14 +03:00

No this persists even with reboot now. Did something in the system change?

Actually, *now* `sws` broke in the terminal too! `sws` is short for

```sh
# in /usr/local/bin/sws
/home/lan/src/cloned/gh/AdrienLeGuillou/sway_window_swithcher_dmenu/sws.sh --dmenu-cmd wmenu
```

It works from when run from the project directory

Also we should add this to a system update script, since I maintain anything under `~/src/`.  We can probably do this when we [[005 Setup a cron job to update system weekly for new gentoo system]]

Maybe these errors are coming from `jq`.

Actually it's from `swaymsg`, this possibly bogus command I am trying out emits it:

```sh
swaymsg [cond_id=#20] focus

# out
Error: Unknown/invalid command 'c'
```

```sh
# in /home/lan/src/cloned/gh/AdrienLeGuillou/sway_window_swithcher_dmenu/sws.sh
CON_ID=${CON_ID##*(}
CON_ID=${CON_ID%)}

echo id: $CON_ID
```

This gives us something like `id: 20`.

I am also able to switch to that window directly with `swaymsg [con_id=20] focus`. And when I am in `~` this same command breaks!

I see now. In `$HOME` I have been creating tmp files that I reuse often for testing and things. I call them `~/a`, `~/b`, and `~/c`. Very easy to remember.

Well `~/c` messes with swaymsg! When I delete it, the command works. That said, if you do `swaymsg "[cond_id=20]" focus`, then it works. Time to file an issue. A hint from here:

https://linuxjunkies.org/guides/linux-globbing-and-extended-glob

It is otherwise a glob pattern:

```sh
ls [ab]

# out
a  b
```

My a and b temporary files...

2026-08-02 Wk 31 Sun - 23:32 +03:00

It hasn't been updated in 6 years. Let's fork it and do a PR.

https://github.com/AdrienLeGuillou/sway_window_swithcher_dmenu/issues/2

```sh
git clone git@github.com:LanHikari22/sway_window_swithcher_dmenu.git ~/src/forked/gh/LanHikari22/AdrienLeGuillou/sway_window_swithcher_dmenu

12 in /home/lan/src/forked/gh/LanHikari22/AdrienLeGuillou/sway_window_swithcher_dmenu/.git/config {
	[remote "upstream"]
	    url = git@github.com:AdrienLeGuillou/sway_window_swithcher_dmenu.git
		fetch = +refs/heads/*:refs/remotes/origin/*
# }
git checkout -b fix-swaymsg-unescaped-glob


```

```sh
# in /home/lan/src/forked/gh/LanHikari22/AdrienLeGuillou/sway_window_swithcher_dmenu > br fix-swaymsg-unescaped-glob
git commit

# out
[fix-swaymsg-unescaped-glob d7aca67] quote-escape cond_id to prevent treated as glob
```

Also get it in my own `fork-main` branch. 

Here's the PR: 

https://github.com/AdrienLeGuillou/sway_window_swithcher_dmenu/pull/3

2026-08-02 Wk 31 Sun - 23:49 +03:00

No idea when we will be responded to. But we have `fork-main` which is up to date with upstream and my fixes. Let's switch to it.