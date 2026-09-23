---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [000 require neovim lua scripts recursively down a directory tree anywhere](../howto/000%20require%20neovim%20lua%20scripts%20recursively%20down%20a%20directory%20tree%20anywhere.md)

Spawned in: [^spawn-entry-0edee3](../howto/000%20require%20neovim%20lua%20scripts%20recursively%20down%20a%20directory%20tree%20anywhere.md#spawn-entry-0edee3)

# Journa

## Retracted Resolutions

### Resolution 1

Set the current working directory to the script full file path. Now when requiring a module, it will check that. Specify a require with `direct_childdir/init` corresponding to a file `direct_childdir/init.lua`.

This can apply recursively, so `direct_childdir` can account for all its direct child directories in its `init.lua` and they have their own and so on.

If we put this at the start of an `init.lua`:

````lua
local filepath = debug.getinfo(1, 'S').source:gsub('^@', '')

local full_filepath
if filepath == vim.fs.abspath(filepath) then
    full_filepath = filepath
else
    local rel_filepath = debug.getinfo(1, 'S').source:gsub('^@./', '')
    full_filepath = vim.fs.abspath(rel_filepath)
end

local script_dir = vim.fs.dirname(full_filepath)

vim.api.nvim_set_current_dir(script_dir)
````

We can now just do

````lua
require('init_d/init')
````

Assuming that in our current working directory there is a `init_d/init.lua`.

OK

### Resolution 1 Retraction Reason

2026-08-03 Wk 32 Mon - 19:29 +03:00

Setting `vim.api.nvim_set_current_dir` is not advisable. This is a side effect that can affect system assumptions down the line. Even if we set and unset it, it is not ideal. Going back to search for a more proper way.
