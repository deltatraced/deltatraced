---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-entry-b7c9f8](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-entry-b7c9f8)

# Journal

2026-08-03 Wk 32 Mon - 06:05 +03:00

Right now I have the ability to play by searching the directories under `~/music`, to play directly from history, or from a basic tagged file to search by tags. `yt-dlp` can be used for downloading yt videos.

`TAGS` is basically the path of the file starting from `~/music` and then `|` and tags. Positive tags: `+mytag`. Negative tags: `-mytag`.

`./play`, `./hplay`, and `./tplay` all add to history.

All is started under librewolf since it can play videos and I didn't strictly need a separate video player. Since I label my windows and use a window switcher, I label the window `mus` and then just switch to the `wolf mus` librewolf window.

For use of tools like `yt-dlp` I like to have an index filesystem where files are placed under `yt-dlp/thru/{channel}/by/{artist}`, but this scheme is not required for the tools to work.

`Window Titler` firefox extension can be configured for window title with `alt+w`, `Close Tabs Shortcuts` firefox extension can be configured for a quick close all other with `alt+q`.

# Files

````sh
# in ~/music/play
#!/bin/bash

script_dir="$(realpath $(dirname "${BASH_SOURCE[0]}"))"

song="$(cd $script_dir && fzf +m)"
ec=$?
[ $ec -ne 0 ] && echo "No results." && exit 1

echo "$(date +%s) $script_dir/$song" >> $script_dir/HISTORY

librewolf "$script_dir/$song"
````

````sh
# in ~/music/hplay
#!/bin/bash

script_dir="$(realpath $(dirname "${BASH_SOURCE[0]}"))"

date_song="$(cat HISTORY | fzf +m -s --tac)"
ec=$?
[ $ec -ne 0 ] && echo "No results." && exit 1
song=$(echo "$date_song" | cut -d' ' -f2-)

echo "$date_song" >> "$script_dir/HISTORY"

librewolf "$song"
````

````sh
# in ~/music/tplay
#!/bin/bash

script_dir="$(realpath $(dirname "${BASH_SOURCE[0]}"))"

song_tags="$(cat TAGS | fzf +m -s --tac)"
ec=$?
[ $ec -ne 0 ] && echo "No results." && exit 1
song=$(echo "$song_tags" | cut -d'|' -f1)

echo "$(date +%s) $script_dir/$song" >> $script_dir/HISTORY

librewolf "$song"
````
