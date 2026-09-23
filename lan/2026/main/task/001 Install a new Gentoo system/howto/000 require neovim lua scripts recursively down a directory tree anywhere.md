---
context_type: howto
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/entry/006 Configuring nvim on new gentoo install](../entry/006%20Configuring%20nvim%20on%20new%20gentoo%20install.md)

Spawned in: [^spawn-howto-d871a5](../entry/006%20Configuring%20nvim%20on%20new%20gentoo%20install.md#spawn-howto-d871a5)

Iterations: [008 Iterations for require neovim lua scripts recursively down a directory tree anywhere](../entry/008%20Iterations%20for%20require%20neovim%20lua%20scripts%20recursively%20down%20a%20directory%20tree%20anywhere.md)

# Objective

We want to be able to put configuration in nested folder structures like `init_d/lsp/rust_analyzer/init.lua`, and we want them to be required without the parent having to account for more than their direct child. So `init_d` knows `lsp` and can require `lsp/init`. And `lsp` knows `rust_analyzer` and can require `rust_analyzer/init` and so on recursively down the directory tree.

We shouldn't be limited to places where the runtime expects scripts. This needs to work for a script directory anywhere on the system.

# Resolution

We need to compute the lua script's full directory path. Then we are able to adjust `package.path` to include it. This variable is searched by `require` for loading modules as specified in https://neovim.io/doc/user/luaref/#require().

We also want this to be included in every `init.lua` under every directory under `init_d`, so that it can apply recursively across the directory tree. Put this in the begining of any `init.lua`:

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

package.path = package.path .. ';' .. script_dir .. '/?.lua'
````

Then add one require per direct child directory:

````lua
require('childdir1/init')
require('childdir2/init')
-- ...
````

As long as every directory accounts for its direct children, we account for the entire directory tree.

OK

# Journal

2026-08-03 Wk 32 Mon - 15:14 +03:00

We want to be able to use a `.config.d/` to put specific service settings to. No more biiiig configuration files that do everything. How do we source all lua files recursively there?

This is how I did it in bash:

````sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles/src/common.sh
source_all_under() {
    folder="$1"

    FILES=$(find $folder -type f | grep ".sh$")

    for file in $FILES; do source $file; done
}
````

https://www.reddit.com/r/neovim/comments/reovwj/how_to_require_an_entire_directory_in_lua/

Here instead they recommend to keep an `init.lua` file in every directory. So Let's try that. It's kind of like how we write `mod.rs` files to recursively include inner modules structurally. It isn't too bad. In each `init.lua`, we have lines ~~`require 'dirname'~~` for each subdirectory, which themselves must have an `init.lua`.

Requiring just `dirname` didn't work, but I am able to do `require 'config/init'` which will read `./config/init.lua`. It also assumes `./config` and so this doesn't always run depending on our `pwd`.

This will fail recursively for us as it tries to figure out where these files are.

https://www.lua.org/pil/3.4.html `3.4 - Concatenation` uses `..`.

Although it is better to use `vim.fs.joinpath`: https://neovim.io/doc/user/lua/#vim.fs.joinpath()

https://neovim.io/doc/user/lua/#vim.fs.dirname()

Guided by https://github.com/neovim/neovim/issues/32116 `Lua: get path of current script/module file (like <sfile>), show Lua plugins in :scriptnames #32116`,

````lua
local full_filepath = debug.getinfo(1, 'S').source:gsub('^@', '')
local script_dir = vim.fs.dirname(full_filepath)
````

You can also find it in https://neovim.io/doc/user/lua/#lua-script-location

So far we have

````lua
-- in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init.lua
local full_filepath = debug.getinfo(1, 'S').source:gsub('^@', '') -- this is wrong; it does not guarantee full filepath
local script_dir = vim.fs.dirname(full_filepath)

require(vim.fs.joinpath(script_dir, '/init_d/init'))
````

--/ 2026-08-03 Wk 32 Mon - 17:29 +03:00 | Amend

````lua
local full_filepath = debug.getinfo(1, 'S').source:gsub('^@', '')
````

inside `init_d/init.lua` gives `./init_d/init.lua`. Which is relative to pwd. We need to use https://neovim.io/doc/user/lua/#vim.fs.abspath():

````lua
local full_filepath = vim.fs.abspath(debug.getinfo(1, 'S').source:gsub('^@', ''))
````

Now we get this strange absolute path: `/home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/./init_d/init.lua`

````lua
local rel_filepath = debug.getinfo(1, 'S').source:gsub('^@./', '')
local full_filepath = vim.fs.abspath(rel_filepath)
local script_dir = vim.fs.dirname(full_filepath)
````

This removes the `./` as well, not just the `@` from the debug info.

--/

But this doesn't work. It ends up searching for a local `./home/lan/...` which is wrong. If we do `init_d/init` and `lsp/init`, then one of them will fail to be found from `.`.

https://www.reddit.com/r/neovim/comments/reovwj/how_to_require_an_entire_directory_in_lua/ also mentions setting `runtime`.

https://neovim.io/doc/user/lua/#lua-module-load

https://stackoverflow.com/a/74142966 recommends `vim.opt.runtimepath:append`.

````
lua vim.opt.runtimepath:append(',~/.config/nvim/lua')
````

Appending to the runtimepath this way doesn't seem to help: `vim.opt.runtimepath:append(',' .. script_dir)` but I find that as long as the `init` is relative to the working directory it will import it: `require('init_d/lsp/init')`

https://stackoverflow.com/questions/58793178/how-to-query-neovim-api-for-the-current-working-directory

We could try to be cwd-relative. Or even set it to the current directory with `vim.api.nvim_set_current_dir`.

`rel_filepath` also behaves differently in the cwd init.lua, yielding `@/home/lan/src/cloned/cb/lan22h/dotfiles/etc/nvim/init.lua` to be processed. The `@` remains because I tried to filter out `@./` but this time that isn't the pattern. It actually is a full path. Here is a check to differentiate:

````lua
local full_filepath
if filepath == vim.fs.abspath(filepath) then
    full_filepath = filepath
else
    local rel_filepath = debug.getinfo(1, 'S').source:gsub('^@./', '')
    full_filepath = vim.fs.abspath(rel_filepath)
end
````

Note, `local full_filepath` needs to be declared outside that ifelse or we get nil if it was local inside and used outside.

Thanks to this we're now able to apply this recursively down the directory tree.

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

require('init_d/init')
````

2026-08-03 Wk 32 Mon - 18:28 +03:00

But this requires a side effect of changing the current working directory, which may have bad consequences down the line.

https://stackoverflow.com/questions/73358168/where-can-i-check-my-neovim-lua-runtimepath/74142966#74142966

`vim.o.path = vim.o.path .. script_dir` also has no effect on recursive require.

````lua
print(vim.opt.runtimepath._value)

-- out
/home/lan/.config/nvim,/etc/xdg/nvim,/home/lan/.local/share/nvim/site,/usr/local/share/nvim/site,/usr/share/nvim/site,/usr/share/nvim/runtime,/usr/lib/nvim,/usr/share/nvim/site/after,/usr/local/share/nvim/site/after,/home/lan/.local/share/
nvim/site/after,/etc/xdg/nvim/after,/home/lan/.config/nvim/after
````

Doing `vim.opt.runtimepath:append(script_dir)` really appends to this, making sure to do the commas itself.

But it doesn't check it. It seems it's not updating.

https://neovim.io/doc/user/lua/#lua-module-load did have a note on this. But I got an error trying to do `let &runtimepath = &runtimepath`: `./init_d/init.lua:16: '=' expected near '&'`

https://github.com/neovim/neovim/issues/12577

https://neovim.io/doc/user/luaref/#require() explains that `require` looks in `package.path`.

This seems to work!

````lua
package.path = package.path .. ';' .. script_dir .. '/?.lua'
````

Now we can update each `init.lua` with

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

package.path = package.path .. ';' .. script_dir .. '/?.lua'

require('childdir/init') -- ...
````

2026-08-03 Wk 32 Mon - 19:27 +03:00

Spawn [008 Iterations for require neovim lua scripts recursively down a directory tree anywhere](../entry/008%20Iterations%20for%20require%20neovim%20lua%20scripts%20recursively%20down%20a%20directory%20tree%20anywhere.md) ^spawn-entry-0edee3
