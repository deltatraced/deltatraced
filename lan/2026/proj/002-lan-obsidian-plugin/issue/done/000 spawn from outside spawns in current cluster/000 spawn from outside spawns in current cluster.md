# 1 Problem

`delta-trace i000`

Using `Lan Obsidian Plugin: Spawn Peripheral Note From Outside` ends up creating a note in the current cluster that is spawning. We want it to behave such that given two clusters `A` and `B`, `B` can spawn a note in `A`, be registered as the spawner but has the file in `B`'s collection and points to `B` as the parent cluster core.

This also only happens when spawning from the core note of the outside cluster. It works as expected from a subnote.

# 2 Journal

2026-05-11 Wk 20 Mon - 01:42 +03:00

Code in `/home/lan/src/cloned/gh/LanHikari22/lan-obsidian-plugin`.

This should be the culprit:

````ts
// in /home/lan/src/cloned/gh/LanHikari22/lan-obsidian-plugin/src/spawn_peripheral_note_command.ts
	if (spawner_is_index) {
		opt_index_file = spawner_file;
	} else if (opt_selected_bignote_root_folder) {
		opt_index_file = notecluster.get_core_file_from_cluster_folder(
			opt_selected_bignote_root_folder
		);
	} else {
		opt_index_file = notecluster.get_core_file_from_peripheral_file(
			view,
			spawner_file
		);
	}
````

`opt_selected_bignote_root_folder`, which is the condition for spawning from outside, should take precedence in case it is both true that `opt_selected_bignote_root_folder` and `spawner_is_index`.

2026-05-11 Wk 20 Mon - 02:08 +03:00

Added information for building in a `BUILD.md` file from the notes I had on the project in `lan-setup-notes`.

2026-05-11 Wk 20 Mon - 03:37 +03:00

Issue solved!
