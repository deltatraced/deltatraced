---
context_type: howto
status: done
---

Parent: [lan/2026/main/wikiproc/001 Wiki Proc HowTos/001 Wiki Proc HowTos](../001%20Wiki%20Proc%20HowTos.md)

Spawned by: [lan/2026/main/wikiproc/001 Wiki Proc HowTos/001 Wiki Proc HowTos](../001%20Wiki%20Proc%20HowTos.md)

Spawned in: [^spawn-howto-902207](../001%20Wiki%20Proc%20HowTos.md#spawn-howto-902207)

# Solution

````sh
sed `{lineno}q;d` {filename}
````

This quits after reaching `{lineno}` instead of processing the whole file, and gives us only that line. so `sed '1q;d' some_file` would give us the first line.

This can also be found at `/home/lan/src/cloned/cb/lan22h-experiments/code-examples/lang/sh/sh/ex000_cat_but_get_nth_line` over at https://codeberg.org/lan22h-experiments/code-examples.

# Journal

2026-07-04 Wk 27 Sat - 19:29 +03:00

I keep encountering this one over and over again.
https://stackoverflow.com/questions/6022384/bash-tool-to-get-nth-line-from-a-file
https://stackoverflow.com/a/6022431/6944447

They seem to suggest this is faster than some alternatives.

````sh
sed '10q;d' file`
````

to get the 10th line from a file, and quit at that line instead of reading the whole file.
