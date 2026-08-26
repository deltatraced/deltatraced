---
context_type: howto
status: done
---

Parent: [[lan/2026/main/wikiproc/001 Wiki Proc HowTos/001 Wiki Proc HowTos]]

Spawned by: [[lan/2026/main/wikiproc/001 Wiki Proc HowTos/001 Wiki Proc HowTos]]

Spawned in: [[lan/2026/main/wikiproc/001 Wiki Proc HowTos/001 Wiki Proc HowTos#^spawn-howto-7cec3c|^spawn-howto-7cec3c]]

# What?

We want to be able to cat a file, but obtain only the lines within the range `line >= n && line < m` for some `nth` and `mth` lines.

# Resolution

Results for this are in `/home/lan/src/cloned/cb/lan22h-experiments/code-examples/lang/sh/sh/ex001_cat_but_within_line_range` over at https://codeberg.org/lan22h-experiments/code-examples.

We provide two solutions that use `sed -n "$start,${end}p`. The `two_pos_ranges.sh` solution just specifies values for start and end, while the `pos_neg_ranges.sh` computes the second range as a substraction from the length of the input, to give us a more intuitive n lines from top and m lines from the bottom.

`pos_neg_ranges` can be used with the following:

```sh
function cat_range { inp="$(cat /dev/stdin)" && echo "$inp" | sed -n "$1,$(expr "$(echo "$inp" | wc -l)" + $2)p"; }
```

Where `$1` and `$2` are to be specified as the positive start and negative end line indices.
# Journal

2026-08-27 Wk 35 Thu - 00:31 +03:00

https://unix.stackexchange.com/a/288525

This has multiple uses with `sed`. We explored prior getting the nth line in [[000 Shell Cat Nth Line]]

2026-08-27 Wk 35 Thu - 01:36 +03:00

Since I wanted a more intuitive interface of specifying the lines from the top then the lines from the bottom I settled on

```sh
function cat_range { inp="$(cat /dev/stdin)" && echo "$inp" | sed -n "$1,$(expr "$(echo "$inp" | wc -l)" + $2)p"; }
```
