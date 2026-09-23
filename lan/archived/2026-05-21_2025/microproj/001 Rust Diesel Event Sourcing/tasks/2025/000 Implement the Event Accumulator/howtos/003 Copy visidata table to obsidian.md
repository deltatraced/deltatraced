---
parent: '[[000 Implement the Event Accumulator]]'
spawned_by: '[[009 Fix conceptual bug of not tracking branch information for soft reset]]'
context_type: howto
status: todo
---

Parent: [000 Implement the Event Accumulator](../000%20Implement%20the%20Event%20Accumulator.md)

Spawned by: [009 Fix conceptual bug of not tracking branch information for soft reset](../tasks/009%20Fix%20conceptual%20bug%20of%20not%20tracking%20branch%20information%20for%20soft%20reset.md)

Spawned in: [^spawn-howto-499709](../tasks/009%20Fix%20conceptual%20bug%20of%20not%20tracking%20branch%20information%20for%20soft%20reset.md#spawn-howto-499709)

# 1 Objective

Copy a visidata table to an obsidian table

# 2 Journal

2025-11-22 Wk 47 Sat - 20:18 +03:00

As specified in the spawning context,

via `vd ,` for selecting a row and `vd Edit>Copy>to system clipboard>selected rows` and specifying `csv` and putting the results in a file `a` to be fed to `cat a | csvtomd` for a given table.

You can also do `vd gt gShift+Y csv` then put content in file `a` and `cat a | csvtomd`.
