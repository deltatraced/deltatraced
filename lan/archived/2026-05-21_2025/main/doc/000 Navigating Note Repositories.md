\#lan #docs #external

# 1 Journal

## 1.1 Software

This uses [obsidian](https://obsidian.md/). The repository will need to be cloned and then opened as a vault using it.

## 1.2 Conventions

### 1.2.1 Folder Structure

2025-07-17 Wk 29 Thu - 23:59

The folder structure here is like this:

There are `tasks/` folders that include current tasks. These may have kanban boards with task information in them.

There are also folders labeled with the year, for example `tasks/2025/` or `entries/weekly/2025/`

Those labeled with the year use global IDs, usually triples (until we exceed 999 note anyway!). For `*/weekly/`, their format is different: `Wk {week_no} {triplet_id} {title}`. For example... `Wk 29 000 My First Entry`

There are `*/topics/` which make hierarchical folders for topics. You can know you're in a leaf node when it contains module folders like `/entries/`, `/tasks/`, `/articles/`, `docs/`,  etc.

We have `*/2025/` and also `*/latest/`. For weekly, latest is usually content from the last few weeks until it's no longer relevant and put in `*/2025/` instead. This may not be very necessary, but these files may eventually grow a lot and you'd have to scroll a lot to find relevant content, so I made this `latest/` folder for that purpose.

You can use this latest convention to quickly see the relevant entries for now:

````sh
# in the note repository
ls **/latest/
````

Or across all note repositories in the system:

````sh
for dir in ~/**/latest/; do [ "$(ls -A "$dir")" ] && ls "$dir" | while IFS= read -r f; do realpath "$dir/$f"; done; done
````

### 1.2.2 Time stamps and format

You will notice that entries have some sort of time format like `2025-07-17 Wk 29 Thu - 23:59`

This is to be read as YYYY-MM-DD, then "Wk" and the week number according to [The ISO Week number](https://www.epochconverter.com/weeks/2025), then Mon-Sun, a dash, and HH:MM.

This can be generated using `Ctrl+P Quick Add: Run QuickAdd` and choosing `insert-datetime`.

### 1.2.3 Using Time logging scripts

Time logs can be started, stopped, and reports can be opened or summarized via Alt+k+e. Those are generated according to `scripts/templater/create_commands_for_user.sh`. For example:

````sh
scripts/templater/create_commands_for_user.sh "My UI Name" "username"
````
