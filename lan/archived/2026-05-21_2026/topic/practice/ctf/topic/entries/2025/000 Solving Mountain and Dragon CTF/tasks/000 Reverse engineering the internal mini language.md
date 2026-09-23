# 1 Journal

2025-07-30 Wk 31 Wed - 07:42

So it seems we have a table of functions, and a long string of data being processed by  these functions.

````ts
  while (g_exit_code == 0) {
    g_chr = get_and_adv_tape();

    var cmd_idx = Math.floor(g_chr / 2);
    console.log(`calling cmd @ idx: ${cmd_idx} from tape byte ${g_data_cur}: ${g_chr}`);

    var fn = g_unk_cmds[cmd_idx];
    if (fn) fn();
  }
````

2025-07-31 Wk 31 Thu - 00:31

Spawn [3.11 Reconstructing tape content](000%20Reverse%20engineering%20the%20internal%20mini%20language.md#311-reconstructing-tape-content) ^spawn-task-d3ebdf

### 1.1.1 Pend
